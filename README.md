# Vodoo v0.4.3

Vodoo is a Windows desktop application for organizing Odoo projects,
confirmed employee and team timesheets, working-time attendance analysis,
and locally recorded computer activity. It uses the authenticated Odoo
account's own permissions.

## Installation and status

Vodoo v0.4.3 is a Public-channel Windows 10/11 x64 application. The
self-contained Full installer is named `Vodoo_Setup_v0.4.3.exe` and
the app-files-only incremental ZIP is `Vodoo_Update_v0.4.3.zip`.
These filenames identify required qualified release artifacts,
not files already built by this source commit. Actual published
downloads are available at the
[Vodoo Releases repository](https://github.com/mustafahafaec-spec/Vodoo-Releases)
only after distribution.

The Lite installer remains retired for new releases. Historical
Lite installers already published are not deleted. The Full edition
retains the established per-user AppId/install path. Complete or
cancel pending Odoo changes before upgrading; local recordings,
session identity, archived days, preferences and user settings
must survive ordinary upgrades.

**Runtime support:** Vodoo still targets .NET 8. Microsoft support
ended November 10, 2026; Full installers bundle the runtime, but a
supported-LTS migration requires separately verified compatibility.

## Connect to Odoo

Choose **Connect to Odoo** and enter your own endpoint and credentials.
Odoo 19+ uses JSON-2 with an HTTPS URL and API key; Odoo 16–18 uses XML-RPC
with database, login and API key. The application offers an illustrated
**How to get** API-key guide. The optional remembered connection stores
credentials in Windows Credential Manager, not in ordinary preferences JSON.

Vodoo does not create project tasks or bypass your Odoo account's access rights.

## Employee mode

**Project View** lets you browse accessible Odoo projects, filter and search
tasks, navigate subtasks and maintain favorites.

**Timesheet** supports By Day, By Week and By Month. The selected display
mode is remembered. Draft new time, edit existing entries, or remove entries,
then explicitly **Confirm To Odoo**. Review an uncertain write in Odoo before
resubmitting, to avoid duplicates. Week/Month date headers support copying
recorded time between dates and marking an account-local Day Off or Official
Holiday.

**Analysis** reads confirmed Odoo hours by project and date. It displays
working hours, 8-hour daily targets, day-offs measured in **days**, leave
permissions in hours, and overtime in hours.

The **By Project** and **By Task** tabs offer expandable parent/child
breakdowns: double-click a project to inspect its tasks or a task to see
its contributing projects. The displayed totals use confirmed Odoo
entries; expanding children does not double-count parent totals.
Use **Export Excel** to produce a separately editable chart and
attendance/timesheet workbook. Every export asks whether to group by
Project or Task.

## Manager mode

Use the **Employee / Manager** selector above the main workspace. Manager
access is granted only after Odoo verifies that the authenticated account
has at least one genuine direct report. There is no unrestricted lookup
of other users or offline promotion to Manager privileges.

Manager Timesheet displays a shared Week/Month calendar, separate
project-only tables for each authorized member and a colored daily-total
row for every member. Manager Analysis includes per-member and overall
attendance metrics, team project charts and Excel export.

**Include myself in the team** is an optional setting for authorized
managers; it is not a way for employees without reports to enter Manager mode.
The application's access checks still follow Odoo permissions.

**External Sheet** lets an authorized Manager manually import an encrypted
productive-time export (`.vds`) shared by an employee. Choose **Import
encrypted file**, then provide the shared passphrase. An import is
stored locally under the currently connected manager account; it does
not upload data to Odoo. Select an imported day and use **Delete selected**
to remove it from the local Manager list after confirmation. Delete
also works when there is only one import; the original encrypted `.vds`
file remains on disk. Employee recordings and Manager imports are not
automatically synchronized between devices.

## Attendance policy

The normal target is **8 hours per eligible workday**. Fridays, Saturdays
and locally marked Official Holidays have zero target. All confirmed hours
recorded on one of these exempt days count as **overtime**.

On other eligible days, time over 8 hours counts as overtime. Missing up to
2.5 hours is a Leave Permission. A deficit greater than 2.5 and up to 4
hours is half a day off (0.5 days); more than 4 hours is one day off.
A manually marked full day off counts as one day, while keeping any
confirmed Odoo time separately as worked time.

Attendance classification includes a grace window for late entry:
By Week excludes the current and immediately previous day;
By Month excludes the current and previous two days.
Friday/Saturday and Official Holiday classifications remain exempt.

Timesheet day totals use these colors: green for exactly 8 hours,
bright yellow (`#FFFF00`) for weekday overtime, orange from 6.5 to
under 8 hours, red from 4.5 to under 6.5, and dimmed below 4.5.
Weekends and Official Holidays are dark blue even if recorded time
on those dates counts as overtime.

**Day-off and permission credits are local analysis calculations;** they
are not Odoo leave approvals or automatically submitted timesheet entries.
Official Holiday marks are account-local settings and are **not**
automatically synchronized between computers or employees.

## Excel exports

Employee and Manager reports export **five worksheets** in this order:

1. **Analysis Report:** native editable horizontal bar chart and confirmed
   hours summary. Manager series show separately colored team-member
   contributions and per-member attendance totals.
2. **Breakdown Report:** native horizontal child-contribution chart grouped
   under parent projects or tasks. Manager child bars are stacked by team
   member, and parent headings display their cumulative hours without
   creating duplicate parent bars.
3. **Full Detail:** individual project/task contribution tables, with
   member detail in Manager reports.
4. **Attendance:** Week/Month Timesheet-style date matrix with employee
   or team attendance, targets, day offs, permissions and overtime.
5. **Chart Data:** linked source values for the editable Analysis chart.

Every Excel export prompts for **By Project** or **By Task**, independently
of which Analysis tab is currently selected. Both native chart legends
are placed **above** the plotted bars; workbook titles are merged and
the source sheet values remain editable. Reports contain confirmed
Odoo hours only, not unsent drafts. The Analysis Report is configured
for A4 landscape printing.

**Excel validation notice:** A vertical alignment shift between child
captions and horizontal bars was reported during private testing.
The latest source removes synthetic spacer categories and restores
centered native axis labels, but there is no confirmed successful
desktop Excel or PDF retest. Check bar-label alignment and exported
print views before relying on Breakdown Report for formal deliverables.

## Recording sessions, productivity and privacy

Recording creates a new **work session** on the first start.
The session header places **Session started** and **Session ended**
side by side on the **left**, without a large empty gap. A session
with no final end timestamp displays exactly **Ongoing**.
A temporarily stopped, resumable session displays **Paused at** its
stop time; a finalized session shows its recorded end time.

The **Show collective day work** checkbox replaces the former
All days' sessions tree. OFF: show only one-day session tabs for
sittings containing **60 minutes or more of actually recorded time**
(exactly 60 minutes qualifies). Sessions below one hour are not
shown individually, and none of their time is eligible for new Odoo
conversions.

ON: ignore individual sessions and show the **entire selected day**
in the table, Pie Chart, Stacked Bars and CSV export. Under-hour
sessions are totaled in one non-productive **Idle time / Sessions
under 1 hour** row. This temporary view does not delete or edit
the underlying session records, task history, conversion states
or pending Odoo writes. Excel productive export offers only qualifying
individual sessions. The checkbox remains visible and can be
unchecked while collective-day work is displayed; only the session
tabs disappear. The preference is saved per account, defaults OFF
and does not inherit the old all-days preference.

**Calendar context menu:** Right-click a date in Recording Calendar
(or press the keyboard context-menu key on the selected date) and
choose **Clear day recordings...**. The action applies to the date
clicked, not another currently selected date. It requires typing
`CLEAR` and safely archives local activity rather than deleting it.
Cleared days are recoverable under **Options > Restore recording day**.
Days without recorded activity, dates with pending Odoo conversions
and the currently active recording day cannot be cleared until
those conditions are resolved. No other days or Odoo entries are
modified.

**Historical recordings:** On first opening the Recording tab, Vodoo
migrates older account-local ActivityHistory.json entries into sessions.
It reads existing local Vodoo recording/project diagnostic log timestamps
to identify verified recording start/stop windows. Surviving individually
timed task entries can also reveal distinct sittings separated by at least
45 minutes when log files are missing. Earlier cumulative app/day records
sometimes span multiple sittings without retaining their individual
timeline; those are kept *intact* in clearly identified historical
combined sessions. Vodoo never invents splits or reallocates Odoo time.
The original pre-migration history is preserved under a dated local
backup, and the new session IDs are retained across restarts. This
migration does not upload diagnostic logs or recorded activity.

**Viewing and actions:** With collective mode OFF, table, charts, CSV and
Odoo actions use the selected qualifying session. With it ON, they
show the selected calendar day; Odoo conversions still exclude
short-session work. Excel workbook export lists qualifying sessions
as separate worksheets. The Manager `.vds` format remains day-scoped.

Idle rules are evaluated on each recording tick:
- 30 seconds with no mouse cursor movement;
- 60 seconds without a click/keystroke AND without scrolling;
- 120 seconds without click/keystroke, after recording has run 120
  seconds (even when scrolling or cursor movement continues).

Any of these rules marks the sample idle. A continuous idle stretch
shorter than 45 minutes stays recorded as non-productive. At **45
continuous idle minutes since the last recorded work**, the ENTIRE
trailing idle stretch is removed and the current session ends at its
last recorded work. Work or a verified active meeting starts a new session
after that cutoff. Confirmed background calls retain their priority.

**Stop / exit / reopen:** Stopping recording or actually exiting Vodoo
temporarily pauses the sitting. If recording resumes (including after
Vodoo restarts) **less than 45 minutes after the last recorded work**
and still on the same local date, it keeps the **same session ID and
original start time**. The time between the stop/exit and restart is
counted as non-productive idle. If 45 minutes or more have elapsed
since the last recorded work, the previous sitting ends at the last
recorded work, any trailing idle time is excluded, and Vodoo starts
a **new session**. Merely minimizing the window into the system tray
does not stop the recorder. Restored historic reconstructed sittings
are never reopened or modified. A session does not continue across
a local midnight boundary.

Ordinary applications require five cumulative minutes of recorded
activity before qualifying as productive. Short tasks are merely
grouped in the Table under Various Tasks; users can mark specific apps
or tasks non-productive and choose Show all tasks. The Stacked Bars
time-display filter has been removed: **Pie Chart and Stacked Bars**
now use the same full productive data without hiding short productive
tasks.

**Export Productive Time** opens a session checklist. Select sessions
and export **Excel (.xlsx)**: each checked session gets an individual
worksheet with session timestamps and productive application/task time.
Excel sheet names use a valid `dd-MM-yy Session N` format (Excel
prohibits the brackets and slash used in the on-screen tab names).
The encrypted `.vds` export route remains available in that dialog
for secure Manager External Sheet imports; `.vds` is still day-scoped.

Activity history stores foreground app/window titles and timing, not
keystrokes or screenshot contents. Information may include sensitive
project/document names. Local account-scoped session metadata and
recordings remain on the Windows user's computer. Preferences can
disable recording when the application is open or in the tray.

## Window controls and official updates

Options include theme, hours format, Windows startup, close-to-tray,
recording and optional automatic-update preferences. The system
tray menu restores or explicitly exits the application.

Public **Check Updates** searches the official Public feed and
accepts only newer verified releases. A valid update can show
Update Now/Later, progress and digest/manifest validation before
staged post-exit replacement with recovery/rollback. Automatic
updates are opt-in and off by default. Pending Odoo writes and
active Odoo operations must not force an unsafe restart. Alpha/Beta
tags are excluded from normal Public update discovery.

The update ZIP contains application files only; it does not
download or rerun the Full installer.

## Windows build and Public release qualification

Use Windows 10/11 x64, .NET 8 SDK, Visual Studio 2022, Windows
PowerShell and Inno Setup 6. From the repository root:

```powershell
dotnet build Vodoo.sln -c Debug
dotnet run --project tests/Vodoo.Core.SmokeTests -c Debug
dotnet run --project tests/Vodoo.App.UISmokeTests -c Debug
powershell -NoProfile -ExecutionPolicy Bypass -File scripts/Invoke-ReleasePackaging.ps1 -ExpectedVersion 0.4.3 -PreflightOnly
dotnet build Vodoo.sln -c Release
dotnet run --project tests/Vodoo.Core.SmokeTests -c Release
dotnet run --project tests/Vodoo.App.UISmokeTests -c Release
powershell -NoProfile -ExecutionPolicy Bypass -File scripts/Test-UpdateHelper.ps1
```

A qualified Public Release solution build uses the existing
Full-only pipeline to produce expected files under `Releases/`:
`Vodoo_Setup_v0.4.3.exe`, `Vodoo_Update_v0.4.3.zip`,
`SHA256SUMS.txt` and `BUILD_INFO.txt`.

Those are local build outputs, not included source files.
Before publication, validate setup fresh/upgrade from published
v0.4.2, local data preservation, Odoo manager permissions,
By Day/Week/Month invalid Project/Task editing and subsequent
valid selection, Recording calendar right-click target, typed
CLEAR/restore, pending Odoo safeguards, themes/DPI, hashes,
staged updater restart and rollback. No runtime build or UI
success is claimed merely from source synchronization.

