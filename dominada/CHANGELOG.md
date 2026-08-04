# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/).

## [Unreleased]

## [0.5.0] - 2026-08-03

Rules a table can set, a sheet it can file, and a wrong number it can fix.

### Added

- **The tournament sheet** (*planilla*): a fourth scoreboard view ruled like
  the paper form, exportable as a PNG for a group chat or as a **real PDF**
  for the desk that files it — selectable text, not a picture of text, with
  the blank cells as fillable form fields so a table can keep score in an
  ordinary viewer or print a stack empty.
- **Sheet templates.** Three shipped (casera, torneo, cuatro-lados) and a club
  can duplicate one, rename its columns, put its own name on it and choose
  which sheet the view and the exports use.
- **Who opens the first hand is a rule**, not a constant. The doble seis
  remains the default and what every existing game plays.
- **A refused move now says which rule refused it**, in Spanish. It used to
  arrive as a debug dump, or — on the host's own screen — as nothing at all.
- **Pro mode.** A table can play for penalties, priced per infraction. The app
  never applies one: it notices, and the **other pareja decides** whether to
  claim it or let it go, which is what every federation text says and what no
  automatic enforcement matches. A fine comes off the offender's own score.
  Letting it go is recorded too.
- **Corrections.** A wrong number stays in the log and is marked corrected
  rather than erased — long-press a hand to fix it, and both figures show on
  the scoreboard and on the printed sheet, with the reason. Anybody can
  contest one; the correction still stands and the disagreement is visible,
  because the app is a scorekeeper and not a referee.

### Fixed

- **Choosing the planilla view silently reverted to the card.** The style
  shipped missing from two independent allow-lists, so the preference worked
  until the app restarted and then normalised away with nothing reporting it.
- **Editing a profile reset the rules its form does not show.** The document
  was rebuilt field by field in Dart, so the tranque-count setting was being
  quietly undone by an unrelated edit like a rename.
- **Toggling pip samples forgot who was holding the device.** Every screen
  re-listed all the settings by hand, and that one had stopped passing `me`.
- **The PDF could not print `Árbitro` or `Peña`.** The built-in PDF fonts have
  no Unicode support and drop accented glyphs silently. A font is now embedded
  — downloading one at export time would fail in the club hall, which is
  exactly where a scoresheet gets printed.
- `undo` is refused once a table is shared, and correction offered instead. A
  guest detects a gap forward and has no notion of the log getting shorter, so
  popping an event left every guest holding something the host no longer had.

## [0.4.1] - 2026-08-03

Shared tables, a rivalry worth posting, and playing on the phones.

### Added

- **Remote play**: the app deals, holds each player's tiles privately, and
  shows the board — over the local network at the same table, or through an
  encrypted relay when apart. Commit-and-reveal, so anybody can check the deal
  afterwards.
- **Rivalries and partner chemistry** between parejas, with a shareable card.

### Fixed

- **Hosting failed with `SocketException … errno = 1`.** The app had never
  declared `INTERNET`, which on Android is what allows binding a socket at
  all. Debug builds get it free for hot reload, so it was invisible to every
  test, to desktop, and to debug installs — and broke only a release build on
  a real phone.
- The build had been asking for **`RECORD_AUDIO`**, merged in by the camera
  plugin because it supports video. This app photographs tiles.
- **"Host this table" hung on a spinner forever.** The discovery call never
  called back, and no try/catch can catch waiting forever.
- **"Deal a hand" did nothing.** The host had no frame sink — the deal worked,
  every guest was served, and the screen that pressed the button was never
  told.

## [0.4.0] - 2026-08-02

The first tagged release. Everything before this shipped by installing a
build by hand; the version in `pubspec.yaml` had never been changed from
the one `flutter create` writes.

**Not 1.0.** The pip counter's accuracy has been measured against exactly two
hand-counted photographs, which is not enough to claim anything, and nobody
outside the author has used the app. 0.4 matches what the work actually
amounts to.

### Added

- **iOS.** The app builds for iPhone: the Rust engine, the bridge, the Dart
  and all five plugins compile and link for arm64. There is no signed build to
  hand out — an iOS app is inert without a signature and the signature belongs
  to whoever installs it — so `tools/ios/` carries a setup script, a build
  script and a guide for signing it with your own Apple ID. The unsigned
  `.ipa` attached to this release can be re-signed with a free Apple ID using
  Sideloadly or AltStore, on Windows, with no Mac involved.

- **Who closed the hand.** *Anotar mano* now offers the winning pareja's two
  members, and an explicit **no sé**. It is optional forever: a scorekeeper at
  a loud table should never be stopped from recording a hand because nobody
  remembers, and a guess would be worse than a blank, because every statistic
  built on top would treat it as fact. Hands recorded before this read as
  unknown rather than being attributed to anyone.

- **One photo, both parejas.** A picture of the table shows every hand on it,
  so boxes are now tagged with the pareja they belong to — one colour per
  side, drawn on the boxes and on the dots they caught — and one photo fills
  in both sides' pips. A side nobody marked is left alone rather than being
  set to zero.

- **Photos can be kept and exported** (Ajustes), so the counting can be
  measured against real pictures instead of assumed to work.


### Added

- Contar los puntos con una foto. In *Anotar mano* there is now a camera
  button beside each side's pip field: take a photo of the tiles left in that
  hand, or pick one you already have, and the app counts the dots. The photo
  comes back with every dot it found circled on it, so you can check the count
  by eye instead of trusting it; if the table has tiles already played in
  shot, drag a box around the hand and it counts again with only what you
  marked. The number then goes into the field **and stays editable** — nothing
  is written to the scoreboard until you save the hand yourself, exactly as
  before. The app says how sure it is, and says so plainly when it is not:
  a wrong count that looked confident would be worse than typing the number.
  If there is no camera, or you would rather not give permission, choosing a
  saved photo always works, and so does the keypad.

- Parejas: a section of its own, between Jugadores and Historial. Every pair
  that has ever shared a side is already there — nothing to create, nothing to
  keep up to date, and no pareja at all until two have played two. Tap one to
  see its full record, who is in it, and every game it played, each of which
  opens from there. Each one carries games played and won, win rate, points
  for and against and premios earned, all read straight off the games
  themselves. A pareja shows up under one entry however the two were
  seated, and a game that arrives as a shared copy from someone else's phone
  lands on the same pareja rather than starting a second one, as long as the
  players are linked. You can name a pareja if you want to; a nameless one is
  listed by its players' names, and clearing a name goes back to that. Naming
  or renaming touches no game, and removing a player never removes a pareja.

- A proper menu: the app now opens on a shell with Partidas, Jugadores,
  Parejas, Historial, Perfiles and Ajustes. On a narrow window they sit behind
  the hamburger; on a wide one they stay beside the content as a menu you can
  shrink to icons and open back up. The scoreboard and the new-game flow
  still take the whole window.
- Three scoreboard views, switchable from the scoreboard itself or from
  Ajustes, and remembered for next time: **Tarjeta** (the original — big
  totals, progress to the target and every hand's breakdown), **Libreta**
  (the way it is written on paper: names underlined across the top, a rule
  between the columns, and each hand's points written down the column of
  whoever won it), and **Grande** (totals only, as large as they fit).
  Anotar mano, deshacer, the screen wake-lock and the win announcement work
  the same in all three, and every number shown is one the engine reported.
- Interface preferences are stored alongside the games and profiles, so a
  backup or a copy of that folder carries them too.

- The app follows your device's light or dark setting by default, and Ajustes
  can pin it to light or dark instead. It used to be dark always, whatever the
  device said.

### Fixed

- **Light pips on dark tiles are counted.** Not every set is ivory with black
  dots; plenty are the reverse. The counter only looked for dark dots, so a
  dark set came back a confident **zero** — the worst kind of wrong, being
  silent and plausible. Both are now read.

- **The table is no longer counted as pips.** On a carpet, about a third of
  what was counted was carpet: the shadows between the loops are the same
  size and darkness as dots and pass every test of shape. A pip sits on a
  moulded plastic face, which is flat, and that is now what tells them apart.

- **Marked areas land where you drew them.** A phone stores the picture
  sideways with a tag saying which way up it goes; the counter ignored the
  tag, so every box was applied to the wrong part of the photo.

- **A pareja's name is written the same way everywhere** and no longer loses
  its second member on a narrow screen.


- A game file that cannot be read no longer hides everything else. Previously
  one damaged file — most plausibly a shared copy that arrived incomplete —
  emptied the game list, the history and every player's record, with no way
  to find the file at fault. The rest of your games now load as usual, and
  Historial names the damaged ones and offers to delete them.
- Rule profiles as editable JSON documents: target score, exact-target play,
  sides and players per side, hand-point mode (all remaining pips vs. losers'
  only), blocked-hand tie-break, next-starter rule, and an open list of bonus
  rules (premios). Ships `pr-200` (Puerto Rico 200) and `pr-500-premios`
  (500 with capicúa and chuchazo) as built-ins; user profiles are the same
  document shape and are created by duplicating a built-in.
- Games recorded as append-only event logs with the rule profile snapshotted
  at start, so editing or deleting a profile never alters a finished game.
  Scores, per-hand point breakdowns, the winner, and the next starter are all
  derived by replaying the log.
- Undo of the last recorded hand, and manual score adjustments with a reason.
- Scoreboard sized to read across a table, one-tap hand entry (dominó winner
  or tranque with per-side pips, plus bonus toggles), and screen wake-lock
  while a game is open.
- A player roster: players are saved once and picked by name afterwards,
  each keeping a record of games played and won, win rate, points for and
  against, and premios earned. Games still store the names they were played
  with, so renaming or removing a player never alters a finished game.
- End-of-game copies: a finished game can be shared with everyone who played
  it, and importing a copy recognises the same people — by their player entry,
  or by an optional account link each player may carry (a label you type, not
  a login; nothing is verified and no server is involved). Players a copy
  cannot place are listed so you can add or link them yourself; two people are
  never merged just because they share a name. Importing the same game twice
  replaces it instead of duplicating it.
- Game history with hand-by-hand detail, JSON export and import, and a
  shareable text scorecard.
- Spanish and English interface; Spanish is the source language.
- Windows and Android builds from one codebase, with all scoring performed by
  the Rust engine embedded in the app.
