# Aurora Launcher

SA-MP mobile client (Android) based on the Aurora Launcher source.

## What's in this build

### Buttons like SA-MP Mobile Launcher
- **TAB** button toggles the scoreboard (press again to close it — previously
  the second press did nothing).
- **ESC** button is now visible and cancels textdraw selection (cancels the
  cursor, like the launcher). While the scoreboard is open, ESC closes it.
- **ALT / H / Y / N** buttons behave like the launcher (walk / ctrl-back /
  yes / no). While the scoreboard is open, pressing one of them closes the
  scoreboard instead of triggering its game action.
- Physical keyboard keys TAB / ESC / Y / H / N / ALT do the same. Keys are
  passed through normally while typing in chat or dialogs.

### Switch servers from a file
The client used to always connect to one hardcoded server. Now the server is
resolved in this order (first valid entry wins):

1. `SAMP/settings.json` in the game data folder (same folder as
   `SAMP/settings.ini`):
   ```json
   {"host": "127.0.0.1", "port": 7777}
   ```
   `port` may also be a string (`"7777"`). If the file is missing or
   invalid, it is ignored.
2. `SAMP/settings.ini` `[client]` `host` / `port`.
3. The built-in default server.

So switching servers is just editing `settings.json` — no rebuild needed.

### No CEF / WebView
This source contains no CEF, WebView, or HTML UI, so there is nothing to
strip: it is already a plain client.

## Build

CI builds the debug APK on every push to `main` and attaches it to a
GitHub Release.

Local build:

```sh
./gradlew assembleVer_debugDebug
```

The APK lands in `app/build/outputs/apk/ver_debug/debug/Aurora-debug.apk`.

### Signing
The signing config reads the keystore passwords from Gradle properties or
environment variables (`AURORA_STORE_PASSWORD`, `AURORA_KEY_PASSWORD`,
`AURORA_KEY_ALIAS`). Copy `gradle.properties.example` to `gradle.properties`
(gitignored) and fill in your values, or pass them with `-P`:

```sh
./gradlew assembleVer_debugDebug \
  -PAURORA_STORE_PASSWORD=... -PAURORA_KEY_PASSWORD=... -PAURORA_KEY_ALIAS=aurora0
```

`aurora.jks` is gitignored and intentionally not in this repo. CI builds
are signed with a throwaway key, so a CI APK cannot update an install
signed with the original key — uninstall the original app first.
