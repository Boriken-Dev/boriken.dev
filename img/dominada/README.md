# Images

`icon.png` — the real app icon, copied from the app source. Re-copy it when the
icon changes; it is not generated here.

## Screenshots

Captured from a disposable emulated Android device, from a **profile** build,
with **invented player names** — Zeus and Atenea against Odín and Freya. A
screenshot of a live game shows whoever was actually playing, and this
repository is public. Both of those choices matter; see "How they were taken".

Listed in the order the product pages show them.

| File | Shows |
|---|---|
| `mesa.png` | The table: the row of played tiles, your own hand below |
| `mano.png` | A freshly dealt hand, opened with the double six |
| `partidas.png` | The games screen with a game in play |
| `tablero.png` | The scoreboard, card view, hands broken down |
| `libreta.png` | The scoreboard, notepad view |
| `grande.png` | The scoreboard, large view — what table mode shows |
| `planilla.png` | The official scoresheet with running totals |
| `remoto.png` | The Remote section: open, create or join a table |
| `sala-remota.png` | A remote room: connected to the relay, QR and room code |
| `record.png` | One player's record: games, win rate, points, partners |
| `rivalidad.png` | A rivalry between two teams: history, streak, margin |
| `tarjeta.png` | The shareable rivalry card |
| `jugadores.png` | The players list with records |
| `parejas.png` | The teams list |
| `perfiles.png` | Rule profiles, built-in and custom |

**Dimensions are not uniform, and the pages already say so.** Eleven are
1080x2230 — captured at 1080x2340 with only the navigation bar cropped. Four
(`grande`, `libreta`, `planilla`, `sala-remota`) are 1080x2130: they survive
from an earlier set that also took 100px off the top for a status bar the
emulated device does not have, which ate into app bars and was dropped in
commit `ed3361c`. Recapturing those four would make the set uniform.

The navigation bar is the device's, not the app's, and does not belong in a
picture of *this* app. Crop with `py -3 tools/crop_png.py shot.png --bottom
110` from the app repository — no `--top`.

Every `width`/`height` attribute on the product pages is derived from the real
file so nothing reflows as the images load. **Re-derive them after any
recapture**; a stale pair is invisible until the page jumps.

### How they were taken

**Captured from Waydroid, not from a real phone** (2026-08-04 onwards). That
matters and is the recommendation: the phone and tablet hold real games with
real players' names, and the safest way not to publish one is not to have one
on the device being photographed. Waydroid is disposable, has no lockscreen
in the way, and is the same `arm64-v8a` as the phone.

The app was seeded with demo data — **Zeus and Atenea against Odín and
Freya**, generated through the real engine so the arithmetic on screen is
what the app actually computes — then captured over `adb`:

```
adb exec-out screencap -p > shot.png
```

Two Waydroid-specific traps:

- **`run-as` does not work there** (`setegid(AID_PACKAGE_INFO) failed`), so
  seeding goes through `sudo waydroid shell` as root instead. Push to
  `/data/local/tmp` first, then copy into the app's data directory and
  `chown` it to the app's uid, or the app cannot read what you just wrote.
- **`monkey -p <pkg>` exits with code -5 and starts nothing.** Use
  `am start -W -n dev.boriken.dominada/.MainActivity`, which works.
  `waydroid app launch` fails with "Already tracking a session" when a
  session is running.

**Use a profile build, not release or debug.**

- A **debug** build paints a DEBUG ribbon across the top right corner.
- A **release** build has no relay credential — `relayCredential()` refuses
  to return one when `kReleaseMode` is true, deliberately, because a key
  compiled into a shipped APK is extractable in minutes. So the Remote screen
  in a release build correctly says remote play is unavailable, which is
  honest in the app and useless in a screenshot meant to show the feature.
- A **profile** build has no ribbon and `kReleaseMode` is false, so:

```
flutter build apk --profile --dart-define=DOMINADA_RELAY_KEY=<a signed key>
```

**Dark theme**, set in the app's own Settings screen (Appearance → Theme →
Dark) rather than by writing a preference file, so the shots show a state a
user can actually reach.

### About the QR in `sala-remota.png`

It encodes a real invitation: `dominada://join?r=…&c=casa-…#<key>`. That is
deliberate — a fabricated QR would scan to nothing, on a page that tells
people the code *is* the key.

It is safe to publish because the room is gone. Rooms exist only while
occupied and the relay stores nothing, so the key opens an empty room that no
longer exists. **Do not reuse this reasoning for a room somebody is actually
in**: the same picture taken during a live game would hand every viewer a
seat at it.

Two things that will bite whoever does this next:

- **`adb shell` corrupts binary** with line-ending translation. Use
  `exec-out` for screenshots and for tar streams, or you get truncated files.
- **Check the foreground app before every capture.** A synthetic tap can miss
  and land in another app; capturing then puts someone's private screen in a
  public repository. It has happened. Guard with:
  `adb shell dumpsys window | grep mCurrentFocus` and refuse unless it names
  the app.

On a real device, seeding uses `run-as`, which needs a **debug** build.
Installing a release build afterwards is a different signature, so it needs
an uninstall — which takes the data with it. Seed after installing the
release build, not before. On Waydroid this does not arise: root can write
the data directory whatever build is installed.

### Still missing

**Spanish screenshots.** These are in English because the phone's locale is
English and the app follows the device. The Spanish page currently shows
English screenshots. Fixing it needs either a Spanish-locale device or an
in-app language setting.

### If reused for an app store

Phone screenshots between 320 and 3840 px per side, longest side no more than
twice the shortest. These qualify. Nothing identifying — the repository is
public.
