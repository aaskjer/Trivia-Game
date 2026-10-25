# Changelog

Earlier releases are in the [release notes](https://github.com/aaskjer/Trivia-Game/releases).
The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/).

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
