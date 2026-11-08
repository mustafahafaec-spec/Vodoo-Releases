# Vodoo v0.1.0

**Vodoo** is a .NET 8 / C# WinForms employee-side task and timesheet organizer for Odoo. It operates using the permissions of the signed-in employee. The Manager mode is visible as **Under Construction**; creating Odoo Project tasks is not provided in Employee mode.

## Requirements

- Windows 10/11 x64, Visual Studio 2022 17.8+ and .NET 8 SDK for development.
- For the published self-contained application, a separate .NET runtime installation is not required.
- An Odoo account with API access and permissions for the relevant Project tasks and Timesheets operations. Supports JSON-2 (Odoo 19+) and XML-RPC (Odoo 16–18), both over HTTPS.
- [Inno Setup 6](https://jrsoftware.org/isinfo.php) to compile the installer `.iss` on Windows.

## End-user workflow

Connect to Odoo, then open **Employee → Project View** to browse projects in colored circles, mark favorites with crowns, navigate to tasks and subtasks, and add timesheet drafts against existing tasks. In **Timesheet**, choose a day on the calendar, enter entries in the editable Project / Task / Description / Hours / Date / State grid, or **Match Another Day** to stage eligible entries. Review the staged operations and click **Confirm To Odoo** to submit them. Existing entries can be selected for staged deletion, subject to Odoo permissions. Some submissions may succeed before others fail; do not blindly resubmit unverified entries.

## Light and dark themes

Click **Light Mode** or **Dark Mode** in the main toolbar. The theme is saved locally for the current Windows user and restored on the next launch. Existing connection preferences and saved API key handling remain unchanged. The user-interface theme does not affect Odoo.

## Description signature

On confirmed timesheet creation, the Odoo `account.analytic.line.name` field receives the description followed by the exact marker:

`(Written By VoOdO)`

Vodoo hides that marker when reading existing timesheets, rendering rows, matching days, and generating task-specific description suggestions. The marker is not part of the editable description shown in Vodoo, and is not appended repeatedly. Existing Odoo project-task description text containing the marker is also suppressed from the corresponding Vodoo display. The marker **is intentionally visible in Odoo**, where it identifies records created by Vodoo. It is not added to a task's `project.task.description` when recording time; it is appended to the **timesheet activity description** only.


## Automatic updates

Vodoo checks for newer **published public releases** automatically on startup. Users can also click **Check Updates** in the main toolbar. Update metadata is read only from the [Vodoo-Releases](https://github.com/mustafahafaec-spec/Vodoo-Releases) GitHub Releases feed, not from Odoo or the source repository.

Only strictly newer public versions (for example, v0.1.1 after v0.1.0) qualify. Internal maintenance identifiers, alpha/beta versions, draft releases and prereleases are ignored. The installer must be named `Vodoo_Setup_vX.Y.Z.exe`, hosted at the exact official release URL, and provide a GitHub SHA-256 asset digest. Vodoo verifies the streamed download's size and hash before saving it as a launchable executable. Corrupt or unverifiable downloads are rejected. Verified installers are cached under `%LOCALAPPDATA%\Vodoo\Updates` and checked again before reuse.

**Updating is opt-in:** Vodoo asks before downloading or launching the installer. The tool will not install while an Odoo operation is busy or the user has unconfirmed timesheet drafts or pending deletions. If new draft work appears during download, installation is deferred. The existing per-user Inno Setup installer is launched only after verification; user settings and Windows Credential Manager entries are preserved. GitHub update requests do not include Odoo API credentials.

A released installer must reuse the existing installation AppId and path. If the releases repository has no published installer newer than the installed public version, the application reports that no newer update is available. Source-repository development does not automatically publish an installer into the Releases repository.

## Visual Studio build and Inno Setup

1. Open `Vodoo.sln` and select **Release | Any CPU**.
2. A **Release Build** automatically creates `artifacts\installer\Vodoo_v0.1.0.iss` by running `scripts\Generate-InnoSetup.ps1` on Windows.
3. Run **`Build Vodoo.bat`** to build and publish a self-contained `win-x64` payload into `artifacts\publish\win-x64`, then regenerate the `.iss` for that payload. Alternatively, `Publish Windows EXE.bat` performs publish and script generation.
4. Open `artifacts\installer\Vodoo_v0.1.0.iss` using **Inno Setup 6** and click **Compile**. The installer will be written to `artifacts\installers\Vodoo_Setup_v0.1.0.exe`.
5. The static reference installer script `installer\Vodoo_v0.1.0.iss` is also in source control; it points to the same publish folder using its own relative path. Prefer the generated artifact after a successful publish.

The installer is a per-user installation under `%LOCALAPPDATA%\Programs\Vodoo`, so it does not require administrator rights. The self-contained publish folder includes runtime dependencies; neither source code nor internal Development Notes are installed. The installer does not deliberately remove the local application settings, favorites, theme choice, or Windows Credential Manager entry during uninstall/upgrade.

## Build and QA checks

- `dotnet build Vodoo.sln -c Release`
- `dotnet run --project tests\Vodoo.Core.SmokeTests -c Release`
- `dotnet run --project tests\Vodoo.App.UISmokeTests -c Release`
- Test both themes, both tabs, 970×630 and 1320×790 window sizes, DPI changes, keyboard selection, entry create/readback, suggestions, day matching, and installer install/upgrade/uninstall on Windows before distribution.

**Verification status:** The v0.1.0 source was statically inspected in a non-Windows environment without a .NET SDK, Inno Setup, or access to your Odoo instance. Windows compilation, GUI tests, installer compilation, and live Odoo permission / write tests must still be performed before distributing the installer.

## Project structure

- `src/Vodoo.App` — WinForms UI, themes and navigation.
- `src/Vodoo.Core` — Odoo clients, timesheet rules and local settings.
- `tests/` — core tests and Windows UI smoke tests.
- `scripts/Generate-InnoSetup.ps1` — generates the Inno Setup `.iss` for Release builds.
- `installer/Vodoo_v0.1.0.iss` — source-controlled reference script.
- `Development Notes/v0.1.x` — development history (not distributed with the installer).
- `Releases Notes v0.1.0.md` — public release notes.

Credentials are never added to source or installer scripts. The API key, when the user chooses to remember it, is stored in Windows Credential Manager; nonsecret connection settings, favorites, usage history, and theme are saved under `%LOCALAPPDATA%\Vodoo`.
