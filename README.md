<h1 align="center">Samsung Root Extended</h1>

<p align="center">
  <!-- Stats (Views, Downloads, Followers, Version) -->
  <img src="https://hits.seeyoufarm.com/api/count/graph/badge.svg?url=https%3A%2F%2Fgithub.com%2FNiklas26903%2FSamsungRootExtended&title=Views&style=for-the-badge&color=24292e" alt="Views" />
  <img src="https://img.shields.io/github/downloads/Niklas26903/SamsungRootExtended/total?style=for-the-badge&logo=github&color=2f363d&label=Downloads" alt="Downloads" />
  <img src="https://img.shields.io/github/followers/Niklas26903?style=for-the-badge&logo=github&color=24292e&label=Followers" alt="Followers" />
  <img src="https://img.shields.io/github/v/release/Niklas26903/SamsungRootExtended?style=for-the-badge&color=2f363d&label=Latest%20Version" alt="Latest Version" />
</p>

<p align="center">
  <!-- Environment (Android, Samsung, One UI, Rooted) -->
  <img src="https://img.shields.io/badge/Android-3DDC84?style=for-the-badge&logo=android&logoColor=white" alt="Android" />
  <img src="https://img.shields.io/badge/Samsung-1428A0?style=for-the-badge&logo=samsung&logoColor=white" alt="Samsung" />
  <img src="https://img.shields.io/badge/One%20UI-4A90E2?style=for-the-badge&logo=samsung&logoColor=white" alt="One UI" />
  <img src="https://img.shields.io/badge/ROOTED-D9534F?style=for-the-badge" alt="ROOTED" />
</p>

<p align="center">
  <!-- Feature Connection (Magisk <--- Auto Setup & Manager ---> LSPosed) -->
  <img src="https://img.shields.io/badge/Magisk-00AF91?style=for-the-badge&logo=android&logoColor=white" alt="Magisk" />
  <img src="https://img.shields.io/badge/-%E2%97%80%E2%94%80%E2%94%80-24292e?style=for-the-badge" alt="<---" />
  <img src="https://img.shields.io/badge/AUTO%20SETUP%20%26%20MANAGER-E67E22?style=for-the-badge" alt="Auto Setup & Manager" />
  <img src="https://img.shields.io/badge/-%E2%94%80%E2%94%80%E2%96%B6-24292e?style=for-the-badge" alt="--->" />
  <img src="https://img.shields.io/badge/LSPosed-5573E4?style=for-the-badge&logo=android&logoColor=white" alt="LSPosed" />
</p>

<p align="center">
  <!-- Stars -->
  <img src="https://img.shields.io/github/stars/Niklas26903/SamsungRootExtended?style=for-the-badge&logo=github&color=ffd700&labelColor=24292e&label=Stars" alt="Stars" />
</p>

Samsung Root Extended is a manager-first setup for rooted Samsung / One UI based systems. The APK checks the device, prepares missing building blocks, installs supported modules from this GitHub release, and keeps updates manageable from inside the app.

## Contents

- [Requirements](#requirements)
- [Current Compatibility Notice](#current-compatibility-notice)
- [What The Manager Does](#what-the-manager-does)
- [Download And Install](#download-and-install)
- [Installed Components](#installed-components)
- [Manager Features](#manager-features)
- [Optional Spoof Add-ons](#optional-spoof-add-ons)
- [Technical Logic](#technical-logic)
- [Copyright](#copyright)

## Requirements

Recommended, but optional:

- A rooted Samsung / One UI ROM
- Magisk with Zygisk enabled

Samsung Root Extended is designed so the manager can prepare missing dependencies itself. If Magisk, Zygisk, LSPosed, the work-profile bypass, or the required helper modules are missing, the app shows only the relevant missing step and offers an install, activate, or update action.

## Current Compatibility Notice

The current functional test target is Noble ROM. Other rooted Samsung / One UI custom ROMs are planned, but should be treated as unverified until tested. The project is intended to become One UI based instead of model-locked.

## What The Manager Does

The manager APK is the control center. It is not just a launcher for files. It checks your root stack, installs supported release assets, sets module scopes where possible, refreshes the work-profile UI, and guides you through the setup without manual command lists.

The work-profile extension is disabled by default until you enable or install the relevant part in the app. This keeps the base setup cleaner and lets optional identity features stay optional.

## Download And Install

Download only the manager APK from the latest release:

[Direct manager APK download](https://github.com/Niklas26903/SamsungRootExtended/releases/latest/download/Samsung-Root-Extended-Manager.apk)

After installing the APK, open it and follow the setup screen. The app will ask for root only when needed.

## Installed Components

<details>
<summary><strong>Magisk / Root Stack</strong></summary>

Easy explanation:

Magisk provides the root base that lets the manager install and activate system-level modules.

<details>
<summary>More technical details</summary>

The manager checks root access through `su`, checks Magisk availability and version, and uses Magisk module installation for supported ZIP components. Reboot-required steps are bundled where possible.
</details>
</details>

<details>
<summary><strong>Zygisk</strong></summary>

Easy explanation:

Zygisk is needed so LSPosed modules can hook Android apps reliably.

<details>
<summary>More technical details</summary>

The manager distinguishes internal Magisk Zygisk from compatible standalone Zygisk modules. It warns if both internal and external Zygisk are active, or if multiple external Zygisk providers are detected.
</details>
</details>

<details>
<summary><strong>LSPosed</strong></summary>

Easy explanation:

LSPosed allows optional app-level behavior changes without editing the original ROM files.

<details>
<summary>More technical details</summary>

The manager checks the LSPosed module, the running LSPosed process, and the LSPosed Manager package separately. A hidden or renamed user app is not treated as fatal when the module itself is working.
</details>
</details>

<details>
<summary><strong>Work Profile Bypass</strong></summary>

Easy explanation:

This module helps Samsung / One UI ROMs create and keep a regular Android work profile working, without needing Samsung Secure Folder.

<details>
<summary>More technical details</summary>

The bypass is a Magisk overlay for framework parts involved in managed-profile and DevicePolicy checks. It is mounted at boot and does not directly edit the original ROM files.
</details>
</details>

<details>
<summary><strong>Work Profile Isolation</strong></summary>

Easy explanation:

This optional LSPosed module improves separation between the main profile and the work profile for scoped apps.

<details>
<summary>More technical details</summary>

The module is installed as an APK and scoped through LSPosed where possible. It focuses on account visibility, profile visibility, and runtime return values for selected target apps.
</details>
</details>

## Manager Features

- First setup screen that shows only the currently relevant missing steps.
- Automatic dependency install and update actions from GitHub release assets.
- Reboot bundling for steps that need a restart.
- Two languages: English and German.
- Customizable themes.
- Dashboard with root, Magisk, Zygisk, LSPosed, work profile, bypass, isolation, and spoof status.
- Manage page with reinstall, single removal, and mass removal options.
- In-app bug report flow with diagnostic log attachment.
- GitHub-based release manifest for downloads and future updates.

## Optional Spoof Add-ons

Main Profile Spoof and Work Profile Spoof are optional add-ons. They can change local device identity values for selected contexts, but they are not required for the base work-profile setup.

This area can be a gray zone depending on app, service, region, and platform policy. Use these add-ons only when you understand the tradeoff. They are kept optional so the core work-profile functionality remains separate from identity spoofing.

## Technical Logic

<details>
<summary><strong>Setup checks</strong></summary>

- Root is requested once and reused through a persistent root session while the app is open.
- Magisk is checked through command availability and optional app package detection.
- Zygisk is classified as internal, external, missing, duplicate external, or internal plus external.
- LSPosed module and LSPosed Manager are checked separately.
- Work profile ownership is stored by work-profile name, Android user ID, and manager marker.
- Required setup no longer depends on the optional spoof modules.
</details>

<details>
<summary><strong>Update checks</strong></summary>

- The manager first checks whether required components exist.
- Version checks happen after the existence checks.
- Update actions are shown only for outdated components.
- Downloads come directly from this GitHub repository release assets.
</details>

<details>
<summary><strong>Removal logic</strong></summary>

- Work profile, bypass, isolation, spoof modules, configs, and manager setup data can be removed separately.
- Magisk and LSPosed are not selected by default in the mass-removal tool.
- After removal, the manager returns to the previous management screen and refreshes detected status.
</details>

## Copyright

Copyright (c) Niklas26903. All rights reserved.
