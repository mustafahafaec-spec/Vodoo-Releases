# Vodoo v0.3.0 — Odoo Employee Timesheet Organizer

**Vodoo** is a Windows desktop organizer for Odoo projects, existing tasks, and
employee timesheets. It uses the Odoo permissions of the signed-in employee.
It does not create new Odoo Project tasks in Employee mode.

## Install / upgrade (Windows 10/11 x64)

Choose **one** installer for the same per-user installation:

| Edition | Filename | Requirements |
| --- | --- | --- |
| Full / offline | `Vodoo_Setup_v0.3.0.exe` | Self-contained Windows x64 installation; no separately installed .NET runtime required |
| Lite | `Vodoo_Setup_v0.3.0_Lite.exe` | Requires Microsoft **.NET 8 Desktop Runtime x64** already installed |

Both installers use Vodoo's existing installer identity and the same application
data directory. Installing the new version is intended to preserve existing
Windows-user preferences, favorites and credentials held outside the
application directory. **Unconfirmed timesheet drafts are not guaranteed to
survive an application restart; confirm or discard them before upgrading.**

**Important v0.2.0 → v0.3.0 upgrade:** Download and run one of the v0.3.0
installers manually for this one-time transition. The updater in an existing
v0.2.0 installation expects an EXE installer, while v0.3.0 introduces verified
application-file ZIP updates. An existing v0.2.0 client cannot automatically
upgrade through a ZIP that its updater does not understand.

**Runtime lifecycle:** v0.3.0 still targets .NET 8. Microsoft support for
.NET 8 ended November 10, 2026; a supported-LTS runtime upgrade is required
for long-term deployment. The framework was not changed without a matching
Windows build and UI/installer validation.

## Connect to Odoo

Choose **Connect to Odoo**.

- Odoo 19+: select JSON-2 and provide the HTTPS URL and API key.
  The login field is not required.
- Odoo 16–18: select XML-RPC and provide the database, login and API key.
- The **How to get** tutorial explains how to generate an API key using
  offline illustrated steps. Do not publish or share your own API key.
- **Remember last connection** uses the current Windows user's
  Credential Manager. API keys are not written to preferences JSON.

The Employee interface presents the connected account's name/email, subject
to Odoo's profile-access permissions.

## Employee workflows

**Project View:** Browse accessible projects, navigate tasks/subtasks, search,
filter and save project favorites.

**Timesheet:** Review, draft, edit and delete employee timesheet records.
Use By Day, By Week or By Month. Your last selected display mode is restored
the next time Vodoo opens. Hours display supports H.h and HH:mm formats.

**Suggestions and copying:** Project/task selectors offer ranked type-ahead
suggestions. Match another day's tasks in By Day, and use matrix day-header
copy/paste workflows in By Week or By Month.

**Confirm To Odoo:** Local drafts, edits and deletions are not sent until
explicitly confirmed. Server operations use employee permissions and
read-back verification. If a write result is uncertain, inspect Odoo before
retrying to avoid duplicate entries.

## Window behavior and preferences

Vodoo displays a system-tray icon while running. Minimize the application to
the tray; restoring is available from the tray icon. The **Exit Vodoo** action
fully closes the application, subject to existing unconfirmed-work safeguards.

Under **Options**, configure start with Windows, minimize to tray when closing
the window, hours display format and related desktop preferences. The
close-to-tray preference is enabled by default.

**Recording:** Public Vodoo includes foreground application and window-title
time recording, grouped cumulative task/application totals, a live current-task
counter, and an activity calendar with day-by-day history. Applications appear
after 2 minutes of recorded time; individual tasks after 5 minutes (shorter
tasks accumulate under **Various Tasks**). Apps/tasks sort by recorded
duration, longest first. The recorder excludes idle/desktop activity.

**Privacy and control:** Recording is **enabled by default** while Vodoo is open,
including in the system tray. It stores application names, window titles and
time locally within the current Windows user's Vodoo data, scoped to the
authenticated Odoo account when it can be identified. Window titles can
contain confidential document or web-page names. Open **Options** to disable
continuous recording; use **Start Recording / Stop Recording** in the Recording
tab to control the current session. No keystroke or mouse-click payloads
are saved. The UI records active-application time, not background windows.

**History and Odoo:** Select a date in the Recording calendar. To hide the
selected day's history, choose **Clear History** and type `CLEAR` exactly.
Records remain stored locally and can be restored from **Options →
Restore cleared recording day...**. For sufficiently long recordings, the
context menu can create Odoo timesheets for a selected project and task,
with per-day entries, explicit confirmation and pending-write safeguards.
Use Odoo conversion only when the connected employee has permission to
create timesheets.

## Updates

v0.3.0 introduces an opt-in public ZIP-based update workflow for **future**
official Public releases. After user approval, Vodoo downloads the
`Vodoo_Update_vX.Y.Z.zip` asset from the official
[Vodoo-Releases](https://github.com/mustafahafaec-spec/Vodoo-Releases)
GitHub Release, verifies the asset SHA-256 metadata and ZIP manifest, stages
only allowed application files and offers **Restart Now / Later**.
It is not a replacement for a first-time Full/Lite installer.

Updates are not applied by writing into an application directory while Vodoo
is still running. The updater uses a post-exit helper and a rollback-aware
replacement workflow. If a verification or staging step fails, the
current installed application is retained.

## Support and limitations

- Windows 10/11 x64.
- Odoo 19+ JSON-2 or 16–18 XML-RPC, over HTTPS, subject to the connected
  employee's Odoo access.
- The Lite edition needs .NET 8 Desktop Runtime x64.
- Large Odoo data ranges may be capped; inspect truncation notices before
  acting on aggregated timesheet data.
- Recording is on by default, and window titles may contain sensitive names;
  disable recording under Options when not wanted. Recording history stays on
  the current PC and is not synchronized between devices.
- .NET 8 is out of support as of November 10, 2026.

The About dialog retains the original Vodoo attribution and colored smiley.

## Project layout and building from the solution

The root now contains three places to look when building:

| Location | Contents |
| --- | --- |
| `Installer/` | The two current Inno Setup source templates and their guide |
| `Releases/` | Finished Full installer, Lite installer, verified update ZIP and SHA-256 checksums |
| `Build/` | Temporary published binaries, candidate builds and historical internal patches; Git-ignored |

The application code remains in `src/`, test projects in `tests/`,
build helpers in `scripts/`, and development history in `Development Notes/`.
Old version-specific Inno files and the intermediate installer generator
were removed from the active source tree; Git history retains them.
Existing ignored `artifacts/` folders on individual PCs are not deleted by
Git checkout. To preview a safe local cleanup, run
`powershell -File scripts/Clean-LegacyArtifacts.ps1`; adding `-Apply`
archives recognizable old installers/ZIPs and removes known generated
intermediate folders while leaving unfamiliar user files untouched.

**The only authoritative version number** is `VodooPublicVersion` in the
root `Directory.Build.props`. It controls compiled app/core metadata, the
title-bar version and the Full/Lite/ZIP filenames. Do not edit version
strings in individual `.csproj` files or Inno Setup scripts.

### Build from Visual Studio (recommended)

1. On Windows, install Visual Studio 2022 with .NET 8 desktop tooling and
   **Inno Setup 6** (with `ISCC.exe`).
2. Open the root `Vodoo.sln`.
3. Set **Solution Configuration = Release** and **Platform = Any CPU**.
4. Click **Build → Build Solution**. You do not need a separate installer
   command. When the application project completes, the build generates
   Full and Lite publish candidates, compiles both installers and prepares
   the update ZIP automatically.
5. Look in the root `Releases/` folder for:

   - `Vodoo_Setup_v0.3.0.exe`
   - `Vodoo_Setup_v0.3.0_Lite.exe`
   - `Vodoo_Update_v0.3.0.zip`
   - `SHA256SUMS.txt`
   - `BUILD_INFO.txt` (compare Git source commit and version across PCs)

A **Debug** build intentionally compiles only for development and does
**not** generate release installers. Building the solution in **Release**
is essential on both PCs. If Inno Setup is unavailable or compiled
versions disagree, the Release build fails instead of claiming success
or leaving an apparently new installer containing an old DLL.

Equivalent command-line build: `dotnet build Vodoo.sln -c Release`.
`Build Vodoo.bat` runs that same build and then executes the Core and
WinForms UI smoke tests. The legacy installer/publish batch files are
compatibility shortcuts to that one command, not alternate pipelines.

### Diagnosing a failed Release build

A Release build requires the Inno Setup 6 command-line compiler `ISCC.exe`.
The packaging script checks the PATH, both Program Files folders and
`%LOCALAPPDATA%\Programs\Inno Setup 6\ISCC.exe`. For portable installations,
set the Windows environment variable `VODOO_ISCC_PATH` to its full EXE path.

Check prerequisites without compiling or deleting any build artifacts:

```powershell
powershell -NoProfile -ExecutionPolicy Bypass -File scripts/Invoke-ReleasePackaging.ps1 -ExpectedVersion 0.3.0 -PreflightOnly
```

The release wrapper validates PowerShell syntax before launching the full
build, so malformed scripts produce a useful error even when the main script
cannot start. If packaging fails, Visual Studio reports a specific `VODOO300` error
instead of only the generic `MSB3073` exit code. Open **View → Output →
Show output from: Build** to see the failed stage. The project also writes
`Build/Logs/ReleasePackaging.log` and
`Build/Logs/ReleasePackagingError.txt`, including the original reason.
A failure still fails the Release build; it must not distribute old
Full/Lite installers or ZIP files by mistake.

### Uploading and quality assurance

Only upload generated output from `Releases/` after checking the
checksum file and installing/upgrading the application on Windows.
The update ZIP's exact name is needed by the verified public updater;
it includes a per-file manifest and only the recognized application
files, not the full runtime or arbitrary project files.

A successful solution build does not publish anything to GitHub. The
separate [Vodoo-Releases](https://github.com/mustafahafaec-spec/Vodoo-Releases)
repository remains a separate distribution step. No source, private
credentials, tests, logs or Development Notes are included inside the
installer or update ZIP. The app continues to target .NET 8, which is
out of support; compatibility testing is required for a supported-LTS
upgrade.
