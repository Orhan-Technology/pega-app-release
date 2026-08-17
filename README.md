# Pegah delivery app — release binaries

Build artifacts only. No source lives here.

The ERP repo (`pegah-ltd`) runs on Odoo.sh, where the whole filesystem is
deployed from git — so committing an ~80 MB APK per release there would grow the
history forever. The binary lives here instead, and `pegah-ltd` commits only
`ot_van_loading/static/app/latest.json`, which points at this file.

## Current

| | |
|---|---|
| versionName | 1.1.1 |
| versionCode | 5 |
| size | 83,265,521 bytes |
| sha256 | `73652a85d98076ece30046fdb385c4b028823931e47634db23c7e77c6c08a6be` |

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
