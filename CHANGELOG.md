# Changelog

Earlier releases are in the [release notes](https://github.com/aaskjer/Trivia-Game/releases).
The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/).

## [9.3.0] - 2026-11-10

### Added

- **The Addicted cut is a setting.** Viewers on the addicted list always lost half of every win. *Addicted cut*
  under *Addicted* on the Access page now says how much: 50 as before, 20 for a lighter touch, 0 for none. The
  message for a viewer added to the list names it as `{addictedCut}`; one saved with "50%" written out follows the
  setting too.
- **Rounds at random times, and only once chat has been talking.** Under *Rounds that start themselves* on the Game
  page, *Random time* draws every wait anew between *Every* and *At most*, and *Only after chat lines* holds a round
  back until a platform's chat has written that many lines since its last automatic round. Together with the fixed
  interval these are the three ways a Streamer.bot timer can run.
- **`StartRound` for your own actions.** An Execute C# Method sub-action on the game action starts a round from any
  action: a hotkey, a Stream Deck button, a raid. Optional arguments `triviaPlatform`, `triviaRounds` and
  `triviaCategory`; it sets `triviaStarted` and `triviaStartedOn`. No cooldown and no moderator check apply.

## [9.2.2] - 2026-11-08

### Fixed

- **Every time field says its unit.** Answer time, first reminder, delay between rounds, the blitz times, both
  cooldowns and the overlay's result time carried no unit in the settings window, while the automatic-round fields
  said "(minutes)". They now read "(s)", and those two "(min)".

## [9.2.1] - 2026-11-08

### Added

- **The overlay names the platform.** A chip in front of the question says whether it went to Twitch, YouTube or
  Kick, for streaming to more than one at once, where every platform plays a question of its own. *Which platform it
  came from* under *What it shows* on the Overlay page turns it off.

### Fixed

- **The overlay follows chat even with `&demo=1` in its URL.** The *Try the demo* answer told you to add `&demo=1`
  to the OBS source to place it, and an overlay with it never listened to Streamer.bot: it played its three sample
  questions on their own 10 to 12 second clocks for every stream after, whatever chat was asked. A demo now listens
  too, and the first real round ends the samples for good. The answer now points to *Show a sample round*.
- **The overlay's result is shown for its full time in a run.** The next question waited for *Delay between
  rounds* alone, 5 seconds by default, and cut a 10 second result short. While the overlay is on, it now waits
  until the result, and the leaderboard after it when that is on, has been up for *Result shown for*.
- **Automatic rounds only go where you are live.** A round was offered to every platform with a connected account,
  or with *Only when chat has been active* off to Twitch, YouTube and Kick alike: chats you were not streaming to got
  questions, and the overlay showed whichever platform's question started last over the one your chat was asked.
  The timed action now asks each platform first: Twitch through its API with Streamer.bot's own login, YouTube
  through the broadcasts Streamer.bot monitors, Kick through its public channel page. Where it cannot be told,
  recent chat decides, as before.
- **The check before saving only asks about what you changed.** It named every problem the settings had on every
  save, so *Save & Exit* right after a save asked again about something you had already kept on purpose, such as
  *Only when chat has been active* switched off. It now asks only about the problems the save brings in.

## [9.2.0] - 2026-11-08

### Added

- **Tells you about updates.** Once per Streamer.bot start, about 20 seconds in, one notice names new releases of
  this game, of the other UIFramework games and of UIFramework: in every chat whose broadcaster account is
  connected (Twitch, YouTube, Kick), or as a Windows notification while none is. The links are in Streamer.bot's
  log. *Tell me about updates* on the About page turns it off for this game. The game action is precompiled on
  application start, so the notice starts with Streamer.bot.
- The host block is UIFramework 1.4.0's; the settings window offers the newest UIFramework on its next open.

## [9.1.0] - 2026-11-07

### Added

- **A tour of the settings window.** The first time the window opens, a short tour rings one part after the other
  (the pages, saving, the customise mode) and says what it does; the **?** at the top right shows it again.
- **UIFramework 1.0.0.** Problems the window finds sit behind a red **i** in its header instead of on the About or
  Log page, and *How to play*, *Log* and *About* sit together at the foot of the page list. The settings action
  offers the new framework on its next open.

### Changed

- **Messages from an old `messages.json` show in the settings window.** Before, only the game read that file: the
  window showed the default texts in their place, and a save wrote those over your messages. The messages now move
  into `configs.json` on the first run of the game or of the window, and the file stays as `messages.json.migrated`.
- **The difficulty slider follows the numbers.** A panel under it, *How a round plays*, shows the answer times,
  the blitz times and the spelling tolerance as they stand, and moves the slider to the stop the numbers match, or
  to *Custom* once one of them is your own. Before, the slider opened on *Standard* whatever the saved numbers were.
- **Only the fields the source uses.** *Fetch ahead* shows for opentdb only, and *Mix in my questions* is gone
  while your own questions are the source; before, they stood there greyed out.
- **The settings action is UIFramework's stub 0.2.0.** When the game action hangs, the settings window waits 30
  seconds for its answer, then carries on and can be closed; before, it waited for good. A game that has not
  described itself for a minute no longer locks the settings until Streamer.bot restarts: the next run opens the
  window.
- The host block is UIFramework's current one (1.0.0). It names every type in full, so it no longer depends on the
  action's `using` lines, and it can declare a tour of the window. Nothing changes in the window.

## [9.0.0] - 2026-11-07

The settings window moves into UIFramework for streamer.bot, so it is the same window every chat game of this
family has, in English or German, and it keeps itself up to date. Nothing changes for chat; an existing
`configs.json` and `Trivia-Questions.json` keep working.

### Changed

- Blitz *Chance*, *Subscriber*, *VIP*, the overlay's *Size* and *Panel opacity* and *Result shown for* no longer
  show their unit twice ("(%) (%)", "(s) (s)"): the field adds it itself.
- The host block in `Trivia Game` is UIFramework's current one (0.9.11). The window needs UIFramework 0.9.9 or
  newer for the question table; the settings action fetches it.
- **Your own questions mix with any source.** *Mine + opentdb* is gone from the source dropdown: **Mix in my
  questions** on the Sources page now mixes your list into whichever source is picked (Wikidata, OdeNet, PokeAPI,
  IGDB or opentdb), **Share of mine** says how often. *opentdb only* is just **opentdb**, *My questions only* is
  **Only my questions**. A `configs.json` with the old mixed source is rewritten as opentdb with mixing on.
- **A tidier Sources page.** Source, language and `Check` share one line, the answer of `Check` shows under them;
  only the description of the picked source is shown; the IGDB keys appear only while IGDB is picked, and the
  Client Secret is masked, as is the overlay's WebSocket password. The share of your own questions no longer reads "Mine (%) (%)".
- **The question list is a table again,** edited in place like before UIFramework: one line per question, a click
  into a cell edits it, a checkbox in every row switches it on or off, Delete removes the picked rows, a search box
  above. *Suggested by* is filled by the game and only read here. Needs UIFramework 0.9.9.
- **The settings window is UIFramework's.** `[TG] - Settings` is now UIFramework's generic settings action: it
  fetches `UIFramework.dll` from GitHub on first use (into `UIFramework\lib` next to Streamer.bot) and keeps it
  current; the window itself is declared once in `Trivia-Game.cs`, as the table of settings the game reads, so
  the window and the game can never disagree. The same pages as before, with the Questions tabs as three pages of
  their own: **Questions** (your own list and which of them may be drawn), **Sources** (where questions come from,
  IGDB access, the reachability check) and **Suggestions**. The rail no longer has keyboard shortcuts.
- **German.** With the window's language set to `de` (the pencil, then *Sprache*), every setting, section, page,
  option and message of the window reads in German; the help and about pages stay English.
- **The questions list** keeps its columns: the switch, the question, the correct answer, the wrong answers as
  `a | b | c`, category, difficulty and who suggested it; `Add row`, `Remove row` and `Restore defaults` (the
  example questions) sit under it. The wrong answers are written to `Trivia-Questions.json` as that `a | b | c`
  text from now on; the game reads both that and the old array, and a question keeps `SuggestedAtUtc`.
- **The difficulty dial** is a slider with the same five stops, Relaxed to Brutal, filling in the same six numbers;
  Custom leaves your own numbers alone.
- **The overlay** is written and set up in OBS by the game action now (the window asks it): `Set it up in OBS`,
  `Show a sample round`, `Copy URL`, `Try the demo` and the folder buttons answer in the window's footer; `Set it up
  in OBS` and the sample use the settings as saved, so save first. A source OBS already has is pointed at the
  overlay again without asking.
- **Settings keys.** The window writes `configs.json` with camelCase keys; the old PascalCase keys are read through
  aliases and moved over on the first save. `CooldownGlobalSeconds`, `CooldownUserSeconds`, `VarName`, `UseBotCD`
  and `SendActionCD` are now `globalCooldown`, `userCooldown`, `pointsVariable`, `cooldownUseBot` and
  `cooldownSendAction`. `DarkMode` and `Egg` move into `uiframework.layout.json` next to it, with the language.
- **Custom triggers carry the round.** `Trivia: Round Won`, `Round Timeout`, `Round Skipped` and `Round ForceEnd`
  fire with `roundOutcome`, `roundPlatform`, `roundId`, `roundBlitz`, `roundQuestion`, `roundCorrectAnswer`,
  `roundCategory`, `roundDifficulty`, `roundSource`, `roundAnswers`, `roundWinners`, `roundWinnerCount`,
  `roundPoints` and `roundCurrency` as arguments, and `GetLastRoundResult` (Execute C# Method on the game action)
  hands the same to any other action. The action log's AWARD line names its change (`Change: +…`), which the Log
  page reads.
- **Tools.** `check-settings.ps1` checks the settings table against the game's and the scoreboard's classes on the
  compiled scripts; `run-window-tests.ps1` opens the real window through the generic stub and writes
  `hosts\trivia\schema.json` in the UIFramework repository; `gui-shots.ps1` photographs the window through the
  framework's preview; `check-defaults.ps1`, `TgShotStub.cs`, `WindowTests.cs` and the page tests, which drove the
  old window, are gone.

### Known gaps (the framework's, for a later release)

- The questions list has a search but no difficulty filter and no Refresh, and no confirmation before deleting
  rows; a question suggested from chat while the window is open is lost when the window then saves the list (the
  old window folded it in).
- The suggestion reward is greyed out, not hidden, while it does not apply.
- The overlay URL is not shown on the page; `Copy URL` puts it on the clipboard and says so.
- The help page is static: the paragraphs that summed up the current settings are gone.
- The placeholder chips under a message explain only the placeholders the framework knows.

## [8.2.0] - 2026-10-30

An overlay for OBS. Nothing changes for an install that leaves it switched off.

### Added

- **An OBS overlay.** A browser source that shows the running round: the question with its options,
  numbered the way chat lists them, a countdown with a bar, and how many have answered. When the
  round ends it marks the right answer and shows the winners with their points, then the top of
  the leaderboard. Blitz rounds are marked in red. It listens to Streamer.bot's own WebSocket
  server, so there is nothing to install beside it.
- **An Overlay page in the settings window.** The switch that turns it on, the WebSocket port and
  password, what the overlay shows, and how it looks.
- **`Set it up in OBS`**, the way Battleship and Blackjack do it: through Streamer.bot's OBS
  connection it adds a browser source the size of the canvas to a scene of your choice, created if
  needed, or to the scene that is live. A source of the same name is pointed at the overlay again
  instead of doubled up, which is also how a changed port or password reaches OBS.
- **`Show a sample round`** plays one made-up round on every connected overlay, to see where it sits
  and how it looks in OBS. `Try the demo` plays them in the browser, the way the page is set.
- **What it shows:** the answer count, category and difficulty, the countdown, the winners and the
  leaderboard each switch on and off, plus how long the result stays up and how many places the
  leaderboard lists. Category and difficulty have their own switch, apart from the chat message's.
- **Look:** the question at the bottom or the top, its size from 50 to 150 %, the animations, and
  five colours with a picker — accent, right answer, blitz, text and panel — plus the panel's
  opacity. A save reaches an overlay already open in OBS, and the labels follow the question
  language. The browser source's Custom CSS still wins over all of it.
- The overlay picks a running round back up when OBS reloads it, from a temporary global the game
  keeps per platform (`Trivia.Overlay.twitch`, `.youtube`, `.kick`), and its look from the
  persisted global `Trivia.Overlay.look` the settings window writes on every save.
- URL options for one platform only, the position and language whatever the page says, sample
  rounds without Streamer.bot, and a connection line for troubleshooting.

### Changed

- The settings window has eight pages. Overlay sits after Messages, so Log, How to play and About
  moved to `Ctrl+6`, `Ctrl+7` and `Ctrl+8`.
- The first tab of the Questions page, where your own questions live, is called **Custom List**
  instead of Question bank.
- **A number box left empty, or holding half a number, stops the save.** It used to go in as 0 and
  come out as whatever floor applies - an empty `Cache size` saved as 1, an empty `Base points`
  as 0. Now Save names the fields and writes nothing until they hold a number.
- **The theme button asks before it switches** when there are unsaved changes: save them, drop them
  or stay. It used to save every page on the way without a word. Only the theme flag is written
  now; the rest goes through Save.

### Security

- Everything the overlay shows goes in as text. A suggested question with markup in it is displayed
  as written, never run.
- The answer only reaches the overlay once the round is over, a skip never sends it, and the
  answer count is never broken down by option.
- A colour that is not `#RRGGBB` never reaches the overlay's styles, and the settings window saves
  it as its default.
- The WebSocket password is kept in `configs.json` and goes into the overlay's URL in OBS, because
  the overlay signs in with it. The settings window shows it masked, in its box and in the URL.

## [8.1.1] - 2026-10-25

Hardening of the settings window. `configs.json` is read as it stands; no setting changes.

### Fixed

- **A damaged `configs.json` took the whole settings script down.** The settings were read into a
  static field, so a file that does not parse threw before anything in the script ran: the window
  never opened, and there was no way back except deleting the file by hand. The window now names
  the damaged file, reads the settings back from the copy beside it or falls back to the defaults,
  and keeps the unreadable one as `configs.json.broken`.
- **Every save leaves `configs.json.bak` behind** — the file as it was before that save, which is
  what the window reads back when the live one turns out to be damaged. The custom questions are
  written the same way. A file that does not parse itself is never kept as the backup, so a broken
  one cannot overwrite the last good copy.
- A `configs.json` the game script happens to be holding open is waited out instead of failing on
  the first try, both when reading and when writing.
- **Only one settings window opens.** A second trigger while it was up opened another one with its
  own copy of the settings, and whichever was saved last won. It now brings the open window to the
  front.
- A name typed into a list on the Access page and not finished with Enter or a comma was dropped
  when Save was clicked straight away.
- The cooldowns and the auto-start interval are handed over as one set, so a save can no longer be
  picked up half-written.

### Changed

- The update check runs beside the window instead of in front of it. A GitHub call that times out
  used to hold the window back for its full three seconds; the Update pill appears once GitHub
  answers.
- The Log page reads the log files the first time it is opened, not while the window is being
  built. Every `Trivia-log-*.txt` in the Logs folder used to be read on every opening, for a page
  most openings never look at.

## [8.1.0] - 2026-10-25

Four new question sources, and the fixes a review of all three scripts turned up. `configs.json`
gains three settings and is otherwise read as it stands.

### Added

- **Four new question sources** in the **Question source** dropdown under **Questions → Sources**:
  - **Wikidata (SPARQL)** — capitals, currencies, continents, cities, World Heritage Sites,
    chemical symbols, moons, painters, authors, film directors and composers, built from Wikidata's
    facts. Chat can pick a topic: `!Trivia geography`, also science, art, literature, film, music.
  - **OdeNet (deutsches WordNet)** — German vocabulary: which word fits a definition, which word
    means the same. The word list (about 4 MB) downloads once, the first time a round needs it.
  - **PokeAPI** — types, generations, Pokédex numbers and entries, evolutions.
  - **IGDB** — release years, studios, platforms, and which game came out first. Needs the Client ID
    and Client Secret of a Twitch developer application; the secret field is masked.
- Every new source asks in multiple choice or True/False as the Game page says, draws its wrong
  answers from facts of the same kind, and ranks difficulty by how well known the subject is.
- **Question language** — English or German for Wikidata, PokeAPI and IGDB.
- **Is the source reachable?** checks whichever source is picked, not only opentdb. For IGDB it
  asks Twitch whether the keys are accepted.
- Fetched facts are kept under `Chat Games/Trivia Game/Sources`; Wikidata's sets fill and refresh in
  the background, one query at a time, so a round does not wait for them.
- About lists the new sources with their licences.

### Fixed

- **`!suggest` from chat never worked.** The script listened for a command id the import does not
  ship, so the message fell through to the answer handler. Only the channel point route got in.
  Suggestions now come from the shipped `!suggest` command and from the suggestion reward, side by
  side, and from no other command.
- The **Brutal** difficulty preset turned into Custom on save: its fuzzy tolerance of 0 was raised
  to 1.
- A number counted as a close spelling of another: `1995` was accepted as the option `1994`.
- Cooldown messages printed the seconds left as the PC's culture wrote them, `4,8276` included.
- The save check, the How to play page and the message help named settings by names they no longer
  have — *Require Mod To Start*, *Only Channel Points*, *Announce Each Answer* "on the Game page"
  among them. They use the labels the window shows now.
- The save check warned about an empty own-question list for any source other than opentdb.
- `!TriviaScoreboard` with a `scoreBoard` argument of 0 or less posted a bare header.

### Changed

- **Both** in the source dropdown is now **Mine + opentdb**, since it is no longer the only mix one
  could mean.
- Settings the picked source does not use — Mine (%), Fetch ahead, the fallback, the language — are
  greyed out instead of looking live.
