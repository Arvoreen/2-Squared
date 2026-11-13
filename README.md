# 2048 Android
This game has been published in Google Play and Cafe Bazaar (an Iranian app store). The full source code is now available with permission. Have fun &lt;3

Here's the published game on Google Play: https://play.google.com/store/apps/details?id=com.azurenight.g2048

This project contains:
- Google Play Games services for leaderboards and achievements. You can remove them if you do not need them.
- No ads or other tracking.

## GitHub Actions

- `.github/workflows/android-ci.yml` runs unit tests, Android lint, and a debug build for pull requests targeting `main`. Configure the `main` branch ruleset to require the **Validate** status check before merging.
- `.github/workflows/release-play.yml` runs after pushes to `main`. It repeats the checks, builds and stores a signed AAB as a workflow artifact, then submits it to the Play **internal** track. Set the repository variable `PLAY_TRACK` to another Play track only when you want releases promoted there automatically.
- The build uses Android API 37 and Java 17. Each release gets a unique version code from `ANDROID_VERSION_CODE_BASE` (default `101`), the workflow run number, and run attempt. Its user-visible version name is `ANDROID_VERSION_NAME_BASE.<run number>` (default base `1.2`, producing names such as `1.2.1`). Set the version-code base to the current highest code in Play if it is above 101; change the version-name base to `1.3` or `2.0` when starting a new minor or major version.

Add these repository **Actions secrets** before merging a release to `main`:

| Secret | Value |
| --- | --- |
| `ANDROID_KEYSTORE_BASE64` | Base64-encoded Play upload keystore (one line) |
| `ANDROID_KEYSTORE_PASSWORD` | Keystore password |
| `ANDROID_KEY_ALIAS` | Upload key alias |
| `ANDROID_KEY_PASSWORD` | Upload key password |
| `GOOGLE_PLAY_SERVICE_ACCOUNT_JSON` | Full JSON key for a Google service account with access to this app in Play Console |

Use the same upload key already registered for this Play app. Create a Google Cloud service account, enable the Google Play Android Developer API, create a JSON key, and grant that service account release access to this app in Play Console. On macOS/Linux, encode the keystore with `base64 < upload-key.jks | tr -d '\n'`; in PowerShell, use `[Convert]::ToBase64String([IO.File]::ReadAllBytes("upload-key.jks"))`.
