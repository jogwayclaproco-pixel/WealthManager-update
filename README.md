# Wealth Manager Update Distribution

Public distribution repository for the Wealth Manager Android application.

## Web-only OTA releases

The Android app checks `latest.json` at startup when an internet connection is available. A compatible web bundle is downloaded only after its release metadata and SHA-256 checksum are validated.

The automated OTA publisher creates this structure:

```text
latest.json
releases/
  <buildId>/
    bundle.zip
    release.json
```

- `latest.json` identifies the newest compatible web bundle.
- `release.json` is the immutable metadata record for that build.
- `bundle.zip` contains the contents of the Vite `dist/` directory at its root, including `index.html` and all referenced assets.
- `buildId` is unique to the source commit and CI build.
- Metadata includes the application ID/version, build ID, source commit SHA, UTC build timestamp, bundle version, native compatibility number, ZIP size, download URL and SHA-256 checksum.

Do not manually replace `latest.json` or edit a published release directory. The publisher validates the application, packages the build, publishes the immutable bundle and metadata, then downloads both public files and verifies the published checksum.

The Android client stores downloaded bundles in app-private files, retains the last working bundle, and activates an update only after checksum and archive validation. An unconfirmed activation is rolled back on the next cold app launch. OTA delivery never updates financial records or clears application storage.

## Native Android releases

Changes to Capacitor plugins, Android code, manifests, Gradle configuration, native dependencies or the native compatibility number are not OTA-compatible. They require a new signed APK/AAB distributed through the [GitHub Releases](https://github.com/jogwayclaproco-pixel/WealthManager-update/releases) page or Google Play.

Google Play remains the recommended channel for native application updates. The application source code and publishing workflow are maintained in the separate `WealthManager-Codemagic` repository.
