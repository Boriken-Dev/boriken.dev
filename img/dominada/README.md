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

All are 1080x2130: captured at 1080x2340 and cropped. The status bar shows
which apps have notifications waiting and the navigation bar is the phone's,
not the app's — neither belongs in a picture of *this* app on a public page.
Crop with `py -3 tools/crop_png.py shot.png --top 100 --bottom 110` from the
app repository.

### How they were taken

The app was seeded with demo data, then captured over `adb`:

```
adb exec-out screencap -p > shot.png
```

Two things that will bite whoever does this next:

- **`adb shell` corrupts binary** with line-ending translation. Use
  `exec-out` for screenshots and for tar streams, or you get truncated files.
- **Check the foreground app before every capture.** A synthetic tap can miss
  and land in another app; capturing then puts someone's private screen in a
  public repository. It has happened. Guard with:
  `adb shell dumpsys window | grep mCurrentFocus` and refuse unless it names
  the app.

Seeding uses `run-as`, which needs a **debug** build. Installing the release
build afterwards keeps the data and drops the debug ribbon.

### Still missing

**Spanish screenshots.** These are in English because the phone's locale is
English and the app follows the device. The Spanish page currently shows
English screenshots. Fixing it needs either a Spanish-locale device or an
in-app language setting.

### If reused for an app store

Phone screenshots between 320 and 3840 px per side, longest side no more than
twice the shortest. These qualify. Nothing identifying — the repository is
public.
