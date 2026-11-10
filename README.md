# CleanYourPhone

A lightweight, dark-themed Android launcher starter plus a cautious, computer-side ADB debloating toolkit.

## Current status

This is an initial source-code starter, not a finished commercial-grade cleaner. It includes:
- Android app entry screen with launcher setup shortcuts
- Basic HOME activity with a simple installed-app list (first 30 launchable apps)
- Read-only package inventory scripts
- Explicit-confirmation ADB disable and restore scripts
- Build instructions and safety notes

## Device compatibility

Target device: Micromax N8301 (Android version not yet confirmed).

The project uses `minSdk 23` and Java 17 for compilation. Actual compatibility must be tested on the phone. The `QUERY_ALL_PACKAGES` permission is declared for the app drawer; Google Play distribution has additional policy restrictions for broad package visibility. For personal sideloading, review the permission before installing.

## Requirements

- Android Studio (current stable)
- Android SDK Platform 35
- JDK 17
- For debloating: Android Platform Tools (`adb`)
- USB cable and USB debugging enabled on the phone

## Build APK

1. Install Android Studio and JDK 17.
2. Open this folder in Android Studio.
3. Allow Gradle sync to finish.
4. Select **Build > Build Bundle(s) / APK(s) > Build APK(s)**.
5. Find the debug APK at `app/build/outputs/apk/debug/app-debug.apk`.
6. Transfer it to the phone and install it. You may need to allow installation from that file manager.

Command line (from project root):

```bash
./gradlew assembleDebug
```

On Windows:

```powershell
gradlew.bat assembleDebug
```

If no Gradle wrapper is present, use Android Studio's Gradle sync/build menu or generate the wrapper with an installed Gradle 8.9:
`gradle wrapper --gradle-version 8.9`.

## Set as default launcher

Install the APK, press Home, then choose **CleanYourPhone**. You can also open the app and use **CHOOSE DEFAULT HOME APP**.

## ADB package inventory

1. Enable Developer options: Settings > About phone > tap Build number seven times (wording may vary).
2. Enable USB debugging in Developer options.
3. Connect to a trusted computer and accept the RSA debugging prompt.
4. Run:

```powershell
powershell -ExecutionPolicy Bypass -File .\tools\windows\list-packages.ps1
```

Save the output before changing anything.

## Disable and restore

Only disable packages you positively identify. Never disable core components such as System UI, Settings, Package Installer, launcher/home components, phone/telephony, Google Play services, permission controller, or vendor update/recovery services.

Disable one package at a time:

```powershell
.\tools\windows\disable-package.ps1 -Package com.example.package
```

Restore it:

```powershell
.\tools\windows\restore-package.ps1 -Package com.example.package
```

ADB `pm disable-user` disables a package for the current Android user. It is not the same as deleting its APK from the system partition. A factory reset may restore defaults or re-enable packages. Do not use random debloat lists.

## Important limitations

- A launcher cannot force-stop or uninstall arbitrary apps, and Android may restart background services.
- The Android app does not run ADB commands internally. Debloating is performed from a trusted computer after the user explicitly confirms each package.
- This project does not root the phone, bypass security, or promise permanent removal across factory resets.
- Do not run `pm uninstall --user 0` on unknown packages. Some removals can break calling, updates, boot, setup, or recovery.
- Cache clearing is app-specific and Android controls access to other apps' private data.

## Publish to GitHub

Create an empty repository on GitHub named `CleanYourPhone` (do not initialize it with a README), then from this folder run:

```bash
git init
git add .
git commit -m "Initial CleanYourPhone Android launcher and ADB toolkit"
git branch -M main
git remote add origin https://github.com/YOUR_USERNAME/CleanYourPhone.git
git push -u origin main
```

Replace `YOUR_USERNAME` with your GitHub username.

## Roadmap

- Searchable app drawer and favorites
- App icons and adaptive icon support
- Storage overview using supported Android APIs
- Safer per-package explanation and allowlist
- Device-specific compatibility testing
- Accessibility and larger-screen polish
