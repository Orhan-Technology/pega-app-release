# Pegah delivery app — release binaries

Build artifacts only. No source lives here.

The ERP repo (`pegah-ltd`) runs on Odoo.sh, where the whole filesystem is
deployed from git — so committing an ~80 MB APK per release there would grow the
history forever. The binary lives here instead, and `pegah-ltd` commits only
`ot_van_loading/static/app/latest.json`, which points at this file.

## Current

| | |
|---|---|
| versionName | 1.1.8 |
| versionCode | 12 |
| size | 91,011,950 bytes |
| sha256 | `cd5bd6a9149882a39e06fa2a0812cb5802891eba88e27e5d47ab51504059224f` |

Download URL used by `latest.json` and the in-app updater:

```
https://raw.githubusercontent.com/Orhan-Technology/pega-app-release/main/pegah.apk
```

## Publishing a new build

1. Bump `version:` in `delivery_pos/pubspec.yaml` — the number after `+` is the
   versionCode and it must increase.
2. `flutter build apk --release`
3. Overwrite `pegah.apk` here, commit, push.
4. In `pegah-ltd`, update `latest.json`: `version_code`, `version_name`,
   `sha256`, `size_bytes`. Push so Odoo.sh redeploys.

The app rejects a download whose sha256 does not match `latest.json`, so the
checksum must be recomputed for every build — Flutter output is not
byte-identical between runs.

## Signing

Every APK must be signed with the same key as the one already on the phones, or
Android refuses the update with "App not installed".
