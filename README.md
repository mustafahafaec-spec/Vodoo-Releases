# Vodoo v0.2.0 — Odoo Employee Timesheet Organizer

**Vodoo** is a Windows desktop organizer for Odoo projects, tasks and employee
timesheets. It works with the permissions of your existing Odoo employee account.
It does not create Odoo Project tasks in Employee mode.

## Install Vodoo (Windows 10/11 x64)

Choose **ONE** installer. Both install the same application, share the same
public upgrade identity and keep account-local preferences/credentials intact:

| Edition | Installer filename | When to use |
| --- | --- | --- |
| Full / offline | `Vodoo_Setup_v0.2.0.exe` | Recommended for general distribution. Includes the .NET 8 Desktop Runtime; does not need a separate runtime installation. This is the canonical asset selected by Vodoo's verified public auto-updater. |
| Lite / smaller download | `Vodoo_Setup_v0.2.0_Lite.exe` | Requires **Microsoft .NET 8 Desktop Runtime x64**, already installed on the computer. The installer contains Vodoo and its dependencies but not the Windows desktop runtime. |

The Lite edition is smaller primarily because it does not duplicate .NET on
machines that already have it. The two installers are alternatives, not a
two-stage installation. If your PC does not have the required runtime, choose
**Full**. Alternatively, install the official [Microsoft .NET 8 Desktop Runtime](https://dotnet.microsoft.com/en-us/download/dotnet/8.0)
(x64, Desktop Runtime — not only the ASP.NET Core or base .NET Runtime) before
running Lite.

**Runtime support note:** This version targets .NET 8, for which Microsoft's
support ends **November 10, 2026**. A migration to a supported LTS framework
should be evaluated before continuing long-term public deployment. The
framework target has deliberately not been changed without a compatibility
build and GUI regression test.

## Connect to Odoo

Open **Connect to Odoo**. For Odoo 19+ use HTTPS URL, choose **JSON-2** and
enter your API key; the login/email field is not required for this protocol.
For Odoo 16–18, choose **XML-RPC** and provide the required database, login
and API key.

Use **API Key → How to get** to open an offline five-step tutorial with
screenshots. The fifth sample screenshot has its key value redacted; do not
share or log your own API key.

The footer has **Forget** on the left and **Connect**, then **Cancel** on
the right. All three buttons have equal height. **Remember last connection**
uses Windows Credential Manager to store a key in the current Windows account;
the key is not written to preferences JSON or application logs. **Forget**
deletes that saved local credential, not an Odoo account or server task.

## Employee workflows

- **Project View:** Browse employee-accessible projects and their tasks,
  navigate subtasks, search/filter and mark project favorites.
- **Timesheet, By Day:** Review individual recorded entries, stage new entries,
  edit descriptions/hours/dates and select an existing project/task with
  intelligent type-ahead suggestions.
- **Timesheet, By Week / By Month:** Use a compact matrix with a row per task,
  day totals, hover descriptions, aggregate hours and project/task suggestions
  when adding rows. Friday and Saturday are styled in greys; today's header
  is highlighted separately.
- **Copy and match:** Match tasks from another day (By Day) or copy/paste
  day tasks through date-header context menus (By Week / By Month).
- **Confirm To Odoo:** Drafts, edits and deletions remain local until this
  explicit action. Undo and Clear Edits operate on pending local changes.
  Odoo access restrictions and verification/conflict protection remain active.
- **Preferences:** Light/Dark modes and H.h / HH:mm timesheet display formats.

The **About** window preserves the original Vodoo attribution and colored
smiley, with the text and smiley centered together.

## Supported protocols and limits

- Windows 10 or 11, x64.
- Odoo 19+ over JSON-2 or Odoo 16–18 over XML-RPC, both via HTTPS and subject to
  the connected employee's Odoo permissions and API access.
- The program may cap exceptionally large server result sets to protect
  responsiveness. If a range is truncated, review the displayed status before
  acting on totals.

## Development / build from source

For source development, use Visual Studio 2022 with .NET 8 Windows desktop
tools, .NET 8 SDK, and Inno Setup 6 for installer compilation.

- `Build Vodoo.bat` builds the Release solution, publishes Full and Lite
  variants into separate folders, and generates both Inno Setup scripts.
- `Build Vodoo Installer.bat` builds and compiles both installer executables,
  then runs `scripts/Measure-InstallerSize.ps1` to report actual sizes and
  the measured Lite/Full size reduction.
- `Publish Windows EXE.bat` publishes both application editions without
  compiling an installer.
- `dotnet run --project tests/Vodoo.App.UISmokeTests -c Debug` exercises
  UI smoke assertions on Windows.
- `dotnet run --project tests/Vodoo.Core.SmokeTests -c Debug` exercises
  core protocol and timesheet tests.

The application uses an untrimmed Release publish; trimming Windows Forms and
reflection-based flows has not been validated and must not be enabled simply
to shrink the package. Debug symbols are excluded from installer payloads.
Only published runtime files are packaged; source, Development Notes and
local secrets are not installed.

**Public-release distribution:** The Full installer filename must remain
`Vodoo_Setup_v0.2.0.exe` to match the current SHA-256-verified public updater.
The Lite installer has a separate filename and is a manual optional download.
Preparing this source branch does not itself upload a public installer or
publish a GitHub Release.
