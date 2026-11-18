# Vodoo v0.4.0

Vodoo is a Windows desktop application for organizing Odoo projects,
confirmed employee and team timesheets, working-time attendance analysis,
and locally recorded computer activity. It uses the authenticated Odoo
account's own permissions.

## Installation

Windows 10/11, x64. Choose **one** installer:

| Edition | Installer | Runtime |
| --- | --- | --- |
| Full / offline | `Vodoo_Setup_v0.4.0.exe` | Self-contained |
| Lite | `Vodoo_Setup_v0.4.0_Lite.exe` | .NET 8 Desktop Runtime x64 |

Use the [official Vodoo Releases page](https://github.com/mustafahafaec-spec/Vodoo-Releases)
to find the latest **published** installer. A source branch is not a
published installer. Both editions install per Windows user, with the same
application identity and install location as previous versions.

**Before upgrading**, confirm or discard any pending Odoo timesheet edits.
Application preferences, favorites and locally stored histories are intended
to remain in their Windows-user profile locations during upgrades.
If coming from v0.2.0 or an earlier client, use the full or Lite installer
rather than relying on that older client's installer-based updater.

**Runtime support warning:** .NET 8 reached the end of Microsoft support on
November 10, 2026. The v0.4.0 application and Lite installer retain the
existing .NET 8 dependency for compatibility. Deploy only with awareness
of the support/security implications; a validated migration to a supported
runtime is required for long-term deployment.

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
permissions in hours, and overtime in hours. Use **Export Excel** for an
editable chart and attendance/timesheet workbook.

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

Both Employee and Manager exports include four worksheets in this order:

1. **Analysis Report:** editable project-hour chart and summary; Manager
   reports also include each member's Working, Target, Days Off (days),
   Permissions and Overtime alongside the team total.
2. **Full Detail:** project-summary matrix, with member contributions
   for Manager reports.
3. **Attendance:** member and team totals, followed by a Week/Month
   Timesheet-style date matrix. Manager has separate member/project tables,
   and Employee has project/task rows.
4. **Chart Data:** linked source values for the editable chart, placed last.

These reports use **confirmed** Odoo hours, not unsent drafts. The Analysis
Report is configured for A4 landscape printing. Validate project-name
axis visibility and page layout in desktop Excel before relying on PDF exports;
an earlier Alpha export was reported to omit project labels.

## Recording and privacy

The **Recording** tab measures active application/window time locally.
It supports daily activity history, application icons, task breakdown,
recovery options and authorized conversion of eligible records into
Odoo timesheet drafts. It is enabled by default while Vodoo is running,
including in the notification area, unless disabled under **Options**.

Recording can capture foreground application and window titles that
contain sensitive project, document or website information. Local history
is stored in the current Windows user's scope; review privacy settings
before enabling it on shared workstations.

## Window controls and updates

Options provide hours formatting, start with Windows, close-to-tray,
and recording controls. The tray menu can restore or explicitly exit
the application, with safeguards for pending work.

Official Public updates are retrieved from the Vodoo Releases repository
as an application-file ZIP, validated using the package manifest, and
applied after exit with backup/rollback handling. Update ZIPs are not
standalone installers. Private Alpha and Beta versions are excluded from
ordinary Public update notifications.

## Build and release qualification (Windows)

The authoritative product identity is `Directory.Build.props`.
Use Visual Studio 2022, .NET 8 SDK, PowerShell and Inno Setup 6.

Run from the repository root:

```powershell
dotnet build Vodoo.sln -c Debug
dotnet run --project tests/Vodoo.Core.SmokeTests -c Debug
dotnet run --project tests/Vodoo.App.UISmokeTests -c Debug
powershell -NoProfile -ExecutionPolicy Bypass -File scripts/Invoke-ReleasePackaging.ps1 -PreflightOnly
dotnet build Vodoo.sln -c Release
```

The validated Release build invokes the established packaging pipeline
and should produce these local files in `Releases/`:

- `Vodoo_Setup_v0.4.0.exe`
- `Vodoo_Setup_v0.4.0_Lite.exe`
- `Vodoo_Update_v0.4.0.zip`
- `SHA256SUMS.txt` and `BUILD_INFO.txt`

Inspect and test the actual generated outputs before publication. A GitHub
source branch or successful compilation alone does not publish a release.
