# Android APK build

This repository is Capacitor-ready. It can produce an installable Android APK through the included GitHub Actions workflow.

## Fastest route
1. Upload this project to a GitHub repository.
2. Open **Actions**.
3. Run **Build Android APK** (or push to `main`).
4. When the workflow finishes, open the workflow run and download the artifact named `xauusd-robot-control-debug-apk`.
5. Extract the artifact and install `app-debug.apk` on Android.

## Important
The APK is the controller UI. It does not magically turn Android into an MT5 trading terminal. Real execution still requires the MT5 EA/bridge running on Windows/VPS and a secure API connection.

For live money, use a properly signed release build, secure API authentication, and demo-test the entire command/execution chain first.
