# Copilot Instructions for Winutil

## Project Overview

Winutil is a modular PowerShell utility for Windows system setup, debloating, troubleshooting, and configuration. It is composed of many scripts, organized for maintainability and compiled into a single distributable script via `Compile.ps1`.

## Architecture & Key Patterns

- **Script Organization:**
  - Core logic is split across `functions/` (with `microwin/`, `private/`, `public/` subfolders) and `config/` for data/configuration files.
  - `functions/microwin/` contains scripts for Windows image customization and automation.
  - `functions/private/` holds internal-use scripts (not exposed to users directly).
  - `functions/public/` is for user-facing functions (if present).
  - `xaml/` contains UI definitions for WPF dialogs.
- **Build Workflow:**
  - Run `Compile.ps1` to concatenate and process all scripts into `winutil.ps1`.
  - No admin rights required for build; admin is required to run the final utility.
- **Configuration:**
  - All presets, tweaks, and app lists are JSON files in `config/`.
  - UI and navigation are driven by these config files and XAML layouts.
- **Testing:**
  - Pester tests are in `pester/` (e.g., `configs.Tests.ps1`, `functions.Tests.ps1`).
  - Run tests using Pester in PowerShell: `Invoke-Pester -Path pester/`

## Conventions & Practices

- **Function Naming:**
  - Use `Verb-Noun` format (e.g., `Invoke-WinUtilAssets`, `Install-WinUtilProgramWinget`).
  - Private/internal functions are in `functions/private/`.
- **Data Flow:**
  - Most user actions are routed through main scripts (`main.ps1`, `start.ps1`) and then dispatched to functions.
  - Configuration and state are passed via JSON and PowerShell objects.
- **External Integration:**
  - Uses Chocolatey and winget for app installs (see `Install-WinUtilChoco.ps1`, `Install-WinUtilProgramWinget.ps1`).
  - UI is built with WPF via XAML files.

## Examples

- To add a new tweak, update `config/tweaks.json` and implement logic in `functions/private/Invoke-WinUtilTweaks.ps1`.
- To add a new app, update `config/applications.json` and relevant install scripts in `functions/private/`.
- For new UI elements, edit `xaml/inputXML.xaml` and update related scripts in `functions/`.

## Quick Reference

- **Build:** `pwsh -File Compile.ps1` (PowerShell Core)  
  Alternatively, use `powershell -File Compile.ps1` for Windows PowerShell.
- **Test:** `Invoke-Pester -Path pester/`
- **Main Entrypoint:** `winutil.ps1` (after build)
- **Config Files:** `config/*.json`
- **UI Layouts:** `xaml/*.xaml`
- **Function Scripts:** `functions/**/*`

## Additional Resources

- [Official Documentation](https://winutil.christitus.com/)
- [Contribution Guidelines](https://winutil.christitus.com/contributing/)
- [Discord Community](https://discord.gg/RUbZUZyByQ)

---

_If any section is unclear or missing, please provide feedback to improve these instructions._
