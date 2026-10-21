# Changelog

Earlier releases are in the [release notes](https://github.com/aaskjer/Trivia-Game/releases).
The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/).

## [8.0.0] - 2026-10-21

New settings window, built the way Classic Chat Gamble and the Slot Machine build theirs, plus the
round-handling fixes behind it. `configs.json` is unchanged and is read as it stands.

### Added

- Settings window rebuilt as an icon rail — Game, Questions, Access, Messages, Log, How to play,
  About — with `Ctrl+1`-`Ctrl+7` to switch and `Ctrl+S` to save.
- Viewer suggestions: `!Suggest question | correct | wrong | wrong`, guarded by a channel point
  reward, Streamer.bot groups or mod status. A Suggestions page sets the guard, the category and
  difficulty they arrive under, the per-viewer cap, and whether they are playable at once.
- Chat picks the category: `!Trivia history`, `!Trivia 3 science`, short forms like tv, movies,
  anime, maths. Only categories a round can be drawn from are offered; an unplayable name cancels
  the round rather than substituting another.
- Messages page: all 63 chat lines behind one picker, placeholder chips, and a preview of the
  finished line including how it reads as a `/me`.
- Difficulty dial: one slider for answer times, countdown reminders and spelling tolerance, Relaxed
  to Brutal. Every number stays editable; editing one switches the slider to Custom.
- Header says what starts a round and whether the timer is on; footer counts unsaved changes.
- `Dispose()` stops countdowns and closes the settings window when the action is destroyed, so a
  recompile mid-round no longer leaves the old build posting reminders.

### Fixed

- **Channel point redemptions never started a round.** A redemption carries a reward id and no
  command id, so it fell through to the answer handler. Start rewards are matched on the reward
  now — and the action needs a **Twitch → Channel Point Reward Redemption** trigger, which older
  imports do not carry.
- Round ownership: skip, force end, a stale timeout and a blitz winner each claim the round before
  announcing, first claim wins. Fixes a last-second skip paying out anyway, force end announcing an
  end it did not cause, a skip racing a blitz winner, phantom "remaining rounds cancelled" lines,
  and a force end between two questions wiped out by the next round starting.
- A round that threw while starting or resolving left that platform stuck on "a round is already
  active" until recompile, with its redemption neither refunded nor fulfilled.
- The update check compared release tags as strings, so a build ahead of the latest release was told
  to update back to it. The notice is a header pill now, not a dialog in the way.
- A question suggested from chat while the settings window was open was overwritten by the next
  save, a theme switch included. Saves fold in what arrived meanwhile; deletions stay deleted. Same
  for the addicted list against `!TA`.
- A category holding fewer questions than the cache batch failed the whole round; the game asks for
  a single question instead of giving up.
- The monthly leaderboard reset wiped the board the moment it was switched on. Enabling only arms
  it, and the first wipe lands at the next month boundary.
- **Blocked From Board** was ignored while **Include Bot** was ticked.
- Close spellings were rejected while normalized matching was off: `Paris.` was turned away while
  the genuine typo `parus` was accepted.
- **Countdown Start** was raised to at least half the answer time. The configured value is what
  chat gets; only the round's own length still caps it.
- An empty **Number Separator** printed `1,234,567` in the game and `1234567` on the leaderboard.
- Action log: series entries carried no platform, the payout column stayed empty for currencies
  with a space in the name, and the filter now knows series, force ends, suggestions and addicted.
- Closing with the title bar's X discarded everything without asking, and question list edits were
  not counted as unsaved changes.
- Opening or closing the settings window restarted the auto start countdown, Cancel and recompiles
  included.
- A round resolving in the background read the addicted groups and the platform from whichever chat
  last sent a message, not from the round's own platform.
- `!ForceEndTrivia`, the addicted command and `!Suggest` answered on switched-off platforms.
- The currency lookup logged an info line for every chat message.
- A redeemed reward the game does not recognise was dropped silently. The usual cause — a start
  reward wired up while **Only Channel Points** is off — logs and refunds now.
- A multi-round redemption with multiple rounds switched off played one round without a word.
- `90s Music` started 90 rounds of a category called `s Music`.
- Settings were re-read and re-parsed on every chat message; now only when a file changes.

### Changed

- **The platform tick-boxes are gone.** Twitch, YouTube and Kick all play, and the reply goes to
  the chat the command or redemption came from. To leave a platform out, untick it on the Trivia
  commands in Streamer.bot. Channel point rewards stay Twitch-only.
- An opentdb outage says so. The log repeats what their server answered instead of a bare
  "403 (Forbidden)", and **Questions → Sources** asks opentdb and reports whether it answers.
- Command names on the How to play page and in the "no username given" message match what the
  import ships: `!Trivia`, `!SkipTrivia`, `!ForceEndTrivia`, `!TA`, `!TriviaScoreboard`, `!TStats`,
  `!TSS`.

### Notes

- `!Suggest` ships with the import. An install that updates only the code has no such command and
  the chat half of suggestions stays silent; the channel point reward works either way.
