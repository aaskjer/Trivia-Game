# Changelog

Earlier releases are in the [release notes](https://github.com/aaskjer/Trivia-Game/releases).
The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/).

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
