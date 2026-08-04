# Images

`icon.png` — the real app icon, copied from the app source. Re-copy it when the
icon changes; it is not generated here.

## Screenshots

Captured from a real phone, release build, with **invented player
names** — Zeus and Atenea against Odín and Freya. A screenshot of a live game
shows whoever was actually playing, and this repository is public.

| File | Shows |
|---|---|
| `partidas.png` | The games screen with a game in play |
| `tablero.png` | The scoreboard, card view, hands broken down |
| `libreta.png` | The scoreboard, notepad view |
| `grande.png` | The scoreboard, large view — what table mode shows |
| `planilla.png` | The official scoresheet with running totals |
| `remoto.png` | The Remote section: open, create or join a table |
| `jugadores.png` | The players list with records |
| `parejas.png` | The teams list |
| `perfiles.png` | Rule profiles, built-in and custom |

All are 1080x2130: captured at 1080x2340 and cropped. The status bar shows
which apps have notifications waiting and the navigation bar is the phone's,
not the app's — neither belongs in a picture of *this* app on a public page.
Crop with `py -3 tools/crop_png.py shot.png --top 100 --bottom 110` from the
app repository.

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

**Use a release build.** A debug build paints a DEBUG ribbon across the top
right corner of every screenshot.

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
