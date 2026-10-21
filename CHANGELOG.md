# Changelog

All notable changes to Trivia Game are documented here.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/).

## [8.0.0] - 2026-10-21

A new settings window, built the way Classic Chat Gamble and the Slot Machine build theirs, and the
round-handling fixes behind it. The settings file is unchanged, so an existing `configs.json` is
read as it stands — but the window it is edited in is laid out differently, and the platform
tick-boxes are gone.

### Added

- **Questions from chat.** Viewers can write trivia for you with `!Suggest` — one message carrying
  the question, the correct answer and up to three wrong ones, separated by pipes. A new
  **Suggestions** page decides who may use it (a channel point reward, Streamer.bot groups, or
  moderators), which category and difficulty suggestions arrive under, how many one viewer may keep
  in the pool, and whether they are playable the moment they arrive. Everything accepted lands on
  the Questions page tagged with the sender's name.
- **Chat can pick the category.** With **Let Chat Pick The Category** on, `!Trivia history` or
  `!Trivia 3 science` plays that category, and short forms like tv, movies, anime and maths work
  too. Only categories a round could actually be drawn from right now are offered, and a name
  nothing can be played from cancels that round rather than quietly substituting another one.
- **A settings window built like the other chat games.** Nineteen entries in six collapsible groups
  gave way to the icon rail Classic Chat Gamble and the Slot Machine use: Game, Questions, Access,
  Messages, Log, How to play and About, with About pinned to the bottom, `Ctrl+1` to `Ctrl+7` to
  switch and `Ctrl+S` to save. The window itself is Classic Chat Gamble's size, and the pages are
  built from its own field vocabulary — sections, rows of labelled fields, real toggle switches,
  and name lists you add to as chips rather than as comma-separated text. The page search and the
  dimmed marks for switched-off features went with the old sidebar.
- **Every chat message behind one picker.** The Messages page lists all 63 of them grouped by when
  they are sent, with the placeholders each one accepts as chips you can click into the text, and a
  preview of the finished line — including how it looks sent as a `/me`.
- **A difficulty dial.** One slider sets the answer times, the countdown reminders and the spelling
  tolerance together, from Relaxed to Brutal. Every number stays editable underneath; changing one
  moves the slider to Custom and remembers what you typed.
- **State at a glance.** The window header says what starts a round and whether the timer is on,
  and the footer says how many changes are unsaved.
- **Cleanup when the action is destroyed.** Recompiling in the middle of a round used to leave that
  round's countdown running in the old build, still posting reminders and still resolving against a
  session nothing else could see. Countdowns are now stopped on the way out, and an open settings
  window closes with it instead of writing settings on behalf of a build that is gone.

### Fixed

- **Channel point redemptions could not start a round at all.** A redemption carries a reward id
  and no command id, and the start handler was only ever reached through the command dispatch, so
  every redemption fell through and was treated as a chat answer. The start rewards are now matched
  on the reward itself, the way the suggestion reward already was.
  This also needs a **Twitch → Channel Point Reward Redemption** trigger on the Trivia Game action,
  which older imports did not carry — take the new import string, or add it once under Actions →
  Trivia Game → Triggers.
- The settings window offered an update every time it opened, because it compared release tags as
  plain strings: a build ahead of the latest release was told to "update" back to it. Versions are
  compared as versions now, and the notice is a pill in the window header rather than a dialog in
  the way of it.
- A question suggested from chat while the settings window was open was silently overwritten the
  next time that window saved — including a save as innocent as switching the theme. Saving now
  folds in whatever arrived meanwhile, while questions deleted in the window stay deleted. The same
  applies to the addicted list: an untouched box no longer pushes a stale list back over what the
  addicted command wrote from chat.
- A category holding fewer questions than the cache batch failed the whole round. opentdb answers
  "no results" when the batch is bigger than the pool your filters leave — some categories hold
  only a handful of hard questions — so the game now asks again for a single question instead of
  giving up.
- Force end was rejected between the rounds of a series, exactly where it is needed most: the next
  question was already on its way, so the run carried on regardless. It now stops a queued run too,
  and a question fetched in the meantime is dropped rather than posted.
- The monthly leaderboard reset wiped the board the moment it was switched on, mid-month. Enabling
  it now only arms it, and the first wipe lands at the next month boundary.
- **Blocked From Board** was ignored whenever **Include Bot** was ticked: the blocked list sat
  inside the bot check, so allowing the bot allowed everyone on that list along with it.
- Close spellings were rejected while normalized matching was off — `Paris.` was turned away while
  `parus`, a genuine typo, was accepted.
- **Countdown Start** was raised to at least half the answer time, so 20 on a 60 second round
  produced a first reminder at 30. The configured value is what chat gets now; only the round's own
  length still caps it.
- An empty **Number Separator** printed `1,234,567` in the game while the leaderboard printed
  `1234567`. Both now read it the way the settings window describes it: empty means no separator.
- Series entries in the action log carried no platform, and the payout column stayed empty for any
  currency with a space in its name. The log filter also knows about series, force ends,
  suggestions and addicted entries now.
- Closing the settings window with the title bar's X discarded everything without asking, and edits
  to the question list were not counted as unsaved changes at all.
- Opening and closing the settings window restarted the auto start countdown even after Cancel, as
  did every recompile of the action. Both now leave a correctly running timer alone.
- A round resolving in the background checked the addicted groups against whichever platform last
  sent a message rather than the platform the round belonged to, and three background tasks read
  the same field for the platform they had been started for.
- `!ForceEndTrivia`, the addicted command and `!Suggest` answered on platforms switched off on the
  General page.
- The currency lookup wrote an info line to the Streamer.bot log for every single chat message.
- Force end could tell chat the run was over and then stop nothing at all. It announced itself
  first and only decided afterwards, so a round that ended in that moment — a timeout, or a blitz
  winner — carried the decision away with it and the remaining rounds played on. Skip, force end, a
  stale timeout and a blitz winner now each claim the round before a word goes out, and whichever
  claims it first is the one that ends it.
- `!SkipTrivia` in the last second of a round announced the skip and then revealed the answer and
  paid out anyway, because the expiring countdown resolved the same round as a timeout.
- A skip and a correct blitz answer arriving together paid a winner about half the time, depending
  on which one reached the round first.
- A round that threw while it was being started or resolved left that platform stuck: every later
  `!Trivia` answered "a round is already active" until the action was recompiled, and the channel
  point redemption behind it was neither refunded nor marked fulfilled.
- A second `!ForceEndTrivia` during the wait between two questions announced another cancelled
  round that did not exist. Force end while a round was still being resolved was worse: chat was
  told the remaining rounds were cancelled and then that the series had finished.
- A force end that landed while the next question was being fetched was wiped out by the round
  starting, so the question went to a chat that had just been told the run was cancelled.
- Redeeming a reward the game does not recognise was dropped without a word anywhere. The usual
  cause — a start reward wired up while **Only Channel Points** is off — now says so in the log and
  refunds the redemption.
- A multi-round redemption with multiple rounds switched off played a single round and said
  nothing, leaving the viewer to work out where the rest went.
- A category with a digit in its name was pulled apart: `90s Music` started 90 rounds of a category
  called `s Music`. The round count and the category are read together now, and only a whole word
  counts as a number.
- The settings were re-read and re-parsed from disk on every chat message. They are now re-read
  only once one of the files actually changes.

### Changed

- The command names shown on the How To Play page and in the "no username given" message now match
  the ones the import actually ships: `!Trivia`, `!SkipTrivia`, `!ForceEndTrivia`, `!TA`,
  `!TriviaScoreboard`, `!TStats` and `!TSS`.
- **The platform tick-boxes are gone.** Twitch, YouTube and Kick all play, and the reply goes to
  whichever chat the command or redemption came from. To leave a platform out, untick it on the
  Trivia commands in Streamer.bot. Channel point rewards stay Twitch-only, because no other
  platform has them.
- An opentdb outage now says so. The log carried only "403 (Forbidden)", which reads like a
  misconfiguration; it now repeats what their server answered, maintenance notice and all. The
  settings window asks opentdb directly under **Questions → Sources** and says whether it is
  answering, so a run of "Failed to fetch" in chat can be told apart from a setting you got wrong.

### Notes

- `!Suggest` has a fixed command id like every other Trivia command, so it arrives with the import.
  An install that updates only the code has no such command, and the chat half of suggestions stays
  silent until the import is taken; the channel point reward for suggestions works either way.

## [7.1.1] - 2026-09-03

### Fixed

- Hotfix for the auto start timed action.

### Added

- An optional second **multi-round channel point reward**: one redemption queues several rounds,
  capped by **Max Rounds Per Redeem**, with viewers able to name the number in the reward's text
  field when it asks for input.

## [7.1.0] - 2026-09-02

### Added

- **Write your own questions.** A new **Own Questions** page under TRIVIA holds an editable
  list of your own trivia — question, correct answer, wrong answers, category and difficulty —
  in the same spirit as Hangman's wordlist. Add, delete, search, filter by difficulty, and
  switch individual questions off without deleting them. **Load Examples** fills the list with
  a dozen questions to start from.
  Everything downstream treats them exactly like an opentdb question: blitz rounds, difficulty
  multipliers, answer matching, the action log and the leaderboard all behave the same.
- **Question Source.** On the same page, a source selector decides where each round's question
  comes from — **opentdb Only** (the default, unchanged behaviour), **My Own Questions Only**,
  or **Mix Both** with a percentage for how often your own list is used.
  **Fall Back To The Other Source** keeps a round alive when the chosen source has nothing to
  give: an empty list, filters that match nothing, or opentdb being unreachable.
- Own questions have their own **difficulty**, **question type** and **categories** filters,
  kept separate from the opentdb ones on the Game Rules page so narrowing one cannot silently
  empty the other. Unlike opentdb, which only reports easy, medium and hard, questions you
  write yourself can be **very hard** — the multiplier for it on the Points page finally has
  something to apply to.
- Your own questions do not repeat until the whole pool that can currently be drawn has been
  used up, the same guarantee opentdb's session token gives.
- The action log now records a **Source** column, so you can see at a glance which rounds ran
  on your own questions and which came from opentdb.
- Saving warns about own-question setups that cannot work: a source that includes your list
  while nothing in it is complete, filters that exclude every question, and a mix set to 0%
  or 100%.
- **Rounds that start themselves.** A new **Auto Start** page under SETUP opens a trivia round
  on its own every so often, so chat still gets questions when nobody remembers to ask for one.
  Set the interval, optionally how many rounds one automatic start plays, and a notice to send
  just before the question — leave that box empty to start silently.
  Nobody is behind an automatic round, so the rules about *who* may start one do not apply to it:
  **Require Mod To Start**, **Only Channel Points** and the start command's cooldowns are all
  skipped. Everything about how the round *plays* is unchanged — question source, answer time,
  blitz, points and the action log all behave exactly as they do for a round somebody started.
- The schedule is a **Streamer.bot timed action**, not a timer hidden inside the script, so it is
  visible under Core > Timed Actions, can be paused there, and survives the action being
  recompiled. The Auto Start page is what switches it on and off and sets its interval — saving
  pushes both straight into Streamer.bot, so you never edit the timed action by hand. Streamer.bot
  only picks a new interval up when a timer is switched off and on again, so saving cycles it for
  you: a change to the interval applies at once rather than after the next restart.
- A turn of the timer that lands while a round is already running is skipped rather than queued,
  so a long series or a round a moderator started never has another stacked on top of it.
- **Only while chat is awake.** Streamer.bot cannot tell a script whether you are live, so Trivia
  uses chat itself as the signal: with **Require Recent Chat** on, a round only starts by itself
  when somebody has spoken within the window you set. Automatic rounds therefore stop on their own
  once the stream ends, and do not fire on the first hello of the next one either. Turning it off
  runs trivia on the interval around the clock, empty chat included.
- One turn of the timer offers a round to every platform ticked on the General page, each judged
  on its own chat and its own running round, the same way a start command typed in one chat only
  affects that chat. They are written to the action log as **AUTO-START**, which is also a filter
  on the Action Log page.

### Notes

- Questions live in `Chat Games\Trivia Game\Trivia-Questions.json`, separate from
  `configs.json` — the list grows without limit, and a broken hand-edit there cannot take the
  rest of your configuration down with it. The settings window is the only thing that writes
  it, and it writes through a temp file and atomic replace like `configs.json`.
- Nothing changes for existing installs until you switch the source: the default stays
  **opentdb Only**, and no question file is created unless you save one.
- Auto Start is **off by default**, and nothing about an existing install changes until you tick
  it. Saving warns about the setups that will surprise you: several rounds per start while Allow
  Multiple Rounds is off, auto starting while Only Channel Points is on, and running with the chat
  requirement switched off.
- The import now carries a timed action named **[TG] - Auto Trivia Timer**
  (`53c3b066-f0fa-4b21-8885-87588c63f4bd`), disabled, repeating, pointing at the Trivia Game
  action. An install that upgrades only the code without re-importing has nothing to drive the
  schedule: ticking Auto Start then logs a warning and does nothing, and the rest of the game is
  unaffected.
- That timed action's own **Lines** field is left at 0 and Trivia never writes to it. Streamer.bot
  treats it as a second condition — set it, and a turn needs that many chat lines since the last
  one *as well as* the interval. It is a blunter instrument than Require Recent Chat, counting
  every line on every platform including blocked users, but the two can be combined if you want
  both.

## [7.0.0] - 2026-08-11

Released as **New GUI & New Features!**

### Changed

- **The settings window now uses a sidebar instead of tab strips.** Tab headers used to run off
  the edge of the window, and half the sections were buried in tabs nested inside the "Other
  Modules" tab. Cooldown, Scoreboard, Channel Points and Action Log are now top-level entries
  in one flat list that cannot overflow.

## [6.1.0] - 2026-08-06

A correctness and cleanup release. Several settings in the GUI were never actually
being saved or read — those are fixed, which means **some options will start taking
effect for the first time**. Please read [Upgrade notes](#upgrade-notes) before going live.

### Fixed

**Settings that silently did nothing**

- Every scoreboard message customisation was ignored. The GUI wrote `ScoreboardHeader`,
  `PersonalStats` and friends, while the leaderboard script read `MsgScoreboardHeader`,
  `MsgPersonalStats`. Names now match, so customised leaderboard messages apply.
- The whole answer-matching section never saved. All six controls — Allow Numeric,
  Allow Exact Text, Allow Normalized Text, Allow Fuzzy Text, Fuzzy Tolerance and
  Min. Fuzzy Length — used tags that could not resolve to a property, so edits were
  discarded on save and the game always ran on built-in defaults.
- Message length settings were never written to `configs.json`, and saving from the GUI
  additionally erased the leaderboard's copy of `YouTubeMaxMessageLength`.

**Chat output**

- Messages sent to Twitch and Kick were never split. A question with four options and a
  long category name can exceed the 500 character limit, and the platform drops it
  silently. All three platforms now split.
- Splitting broke mid-word. It now breaks on the last newline or space before the limit.
- Answering twice in one round produced two contradicting messages — an acceptance
  ("close enough — recorded!") immediately followed by "you already submitted". The
  duplicate check now runs before any match feedback is sent.

**Answer matching**

- Fuzzy matching returned the first option within tolerance rather than the closest one.
  At the default tolerance of 2, a typo one edit away from two different options resolved
  by shuffle order. It now takes the closest option, and rejects genuinely ambiguous ties
  instead of guessing.
- Numeric input shadowed literal numeric answers. On a question whose options are numbers
  ("2", "4", "8", "16"), typing `2` selected *option two* instead of the answer "2".
  Exact text is now matched before numeric indexing.

**Leaderboard**

- `!leaderboard` could crash on duplicate or blank user ids and take the whole command
  down before its own filter ran.
- The monthly reset only enumerated users who had a recorded win, so anyone holding
  points with zero wins kept their totals straight through the reset. It now covers all
  four stat keys.
- The monthly reset ran inline, making whichever viewer happened to trigger it wait for
  a sweep of potentially thousands of database writes. It now runs in the background and
  is single-flighted so overlapping commands cannot start a second pass.

**Settings window layout**

- Tab headers ran off the edge of the window instead of wrapping. Both the top-level tabs
  and the nested Other Modules tabs now wrap onto as many rows as they need, so nothing is
  unreachable at the default 559px width or when the window is made narrower.
- Action log columns were star-sized, which forced them all to share the viewport width and
  squeezed values into truncation. They now size to their content and the grid scrolls
  horizontally, with a per-column cap so one long answer cannot stretch a column off-screen.
- The action log filter bar also wraps rather than overflowing.

**Reliability**

- The action log was written without synchronisation from three different threads.
  Concurrent writes threw and silently dropped entries.
- The opentdb HTTP client had no timeout, so an unresponsive API left a round stuck in
  "starting" for the full 100 second default.
- `!addicted` rewrote the entire `configs.json` from a snapshot taken when the command
  started, discarding anything changed in the settings GUI meanwhile. It now patches only
  its own key.
- Config writes go through a temp file and atomic replace; an interrupted write could
  previously leave `configs.json` truncated and unparseable.
- Rounds ran slightly long. The countdown counted one-second sleeps, but sending chat
  messages takes real time, so the drift accumulated. Remaining time is now measured
  against the round deadline.
- Default values had drifted between the game script and the GUI (base win points,
  require-mod-to-start, number separator). The game script also shipped placeholder
  blocklists containing `someuser,anotheruser` and `MyGroupName,AnotherGroup`, which
  applied literally on a fresh install.

### Added

- **Back-to-back rounds.** Putting a number on the start command queues that many rounds in a
  row — `!trivia 3` plays three, one after another, with a configurable breather between them
  so each result is readable before the next question lands. Chat gets a "Round 2 of 3" header
  and a closing line when the series ends.
  Controlled by **Allow Multiple Rounds** (master switch), **Max Rounds Per Command** (requests
  above it are clamped, not rejected) and **Delay Between Rounds**.
  Skip advances the queue to the next question; force end cancels the whole queue, which is the
  intended way to stop a long series early. A failed question fetch abandons the rest rather
  than hammering opentdb. Channel point redemptions always run exactly one round, since the
  reward is redeemed once.
- **Search across every setting.** A search box at the top of the sidebar filters pages by
  name, group, setting label and setting key. Empty groups fold away, and the view jumps to
  the first page with a match. Ctrl+F focuses it, Esc clears it.
- **Unsaved-changes counter.** Every control is snapshotted when the window opens, so the
  footer reports exactly how many settings differ — not just that something changed. Cancel
  asks before discarding them, and switching theme folds pending edits in rather than
  dropping them.
- **Clickable placeholder chips.** Each message now lists its placeholders as chips beneath
  the box; clicking one inserts the token at the caret, and hovering explains what it holds.
  No more copying `{blitzpoints}` out of a tooltip by hand.
- **Reset Page.** Restores just the page you are looking at to defaults, leaving the rest
  alone and writing nothing until you Save.
- **Save-time sanity check.** Saving a combination that cannot work — no platform enabled,
  every answer-matching mode off, channel-point-only with no reward ID, Award Points on with
  a base of zero — now explains the problem and asks before writing.
- **A "How To Play" page** documenting the commands, the answer-matching order, and what the
  current settings actually add up to.

- **Failed channel point redemptions are refunded.** Twitch charges the viewer the moment
  they redeem, but the round does not always start — one may already be running, the
  command may be on cooldown, or opentdb may fail to return a question. Those redemptions
  are now cancelled so the points go back, and the viewer is told in chat.
  Controlled by **Refund Failed Redemptions** on the Twitch Channel Points tab, and requires
  the reward to keep its redemptions in the request queue: a reward with "Skip Reward
  Requests Queue" enabled is already fulfilled as far as Twitch is concerned and cannot be
  refunded.
- Redemptions that *do* start a round are now marked fulfilled, so they stop accumulating
  in the broadcaster's reward request queue.
- Ticking **Only Channel Points** while **Require Mod To Start** is on now switches the
  moderator requirement off and explains why. The two together reject every redemption
  from a regular viewer.
- **Repeat questions are now prevented.** The game uses an opentdb session token, so the
  API stops serving questions it has already given out. Expired and exhausted tokens are
  recovered automatically. Previously, duplicates were only filtered within a single
  fetched batch.
- **opentdb rate limiting.** Requests are serialised and spaced to respect the one-per-five-seconds
  limit, with backoff and retry on HTTP 429. A cache miss shortly after another fetch now
  waits instead of failing the round with "could not fetch a trivia question".
- **Announce Each Answer** setting. Turn it off on busy channels — one confirmation per
  answer is the fastest way to hit the bot's chat rate limit.
- **Points Awarded (Multiple)** message. A round with several winners previously sent one
  payout message per winner. Winners are now grouped into a single message per distinct
  amount, so sub and VIP boosts still read correctly.
- Leaderboard rows are packed into as few chat messages as fit instead of one message
  per row.
- Seven messages that had no GUI row and could not be edited at all: both cooldown
  messages and all five addicted-list messages.
- Every message in the Messages tab now documents when it fires and lists its placeholders.

### Changed

- **Channel points are trigger-only.** The bet mode selector and its `ChannelPointBetMode`
  and `ChannelPointFixedBet` settings have been removed. They were inherited from another
  script and never did anything — no points were ever wagered, deducted or paid out by them.
  Channel points decide *whether* a round may start, nothing more.
- **`messages.json` is merged into `configs.json`.** One file now holds settings and
  messages. Migration is automatic; see below.
- **The action log rolls over daily.** Entries go to `Chat Games\Logs\Trivia-log-YYYY-MM-DD.txt`,
  a fresh file per day, replacing the single file that grew forever. Old days are kept
  indefinitely — nothing is deleted automatically. The GUI's Action Log page reads every file
  in the folder, so history stays searchable however far back it goes.
- Message length limits are hardcoded per platform (Twitch and Kick 500, YouTube 200)
  rather than being editable. These are platform constraints, not preferences.
- Session state is no longer re-serialised and written on every single answer.
- Fuzzy and normalised matches now send one message instead of two.
- Renamed "Multiple Blitz Winners" to "Winners (Normal Round)" — it is the normal round
  message. A blitz round ends on the first correct answer and can only ever have one winner.

### Removed

- `Stopped` and `NotRunning` messages. Both were saved, shown in the GUI, and never sent
  by anything. There is no stop command by design: a round always ends itself when the
  timer expires, and moderators can force end early.
- `MaxMessageLength` and `YouTubeMaxMessageLength` settings (now hardcoded).
- `AllowChannelPointStart` and `ChannelPointRewardId`, which duplicated
  `IsChannelPointsTrigger` and `TargetRewardId` and were not wired to anything.
- Unused single-question fetch path.

### Upgrade notes

1. **Check your answer-matching settings before going live.** They never saved, so
   whatever the GUI showed you was not necessarily what the game was using. Open the
   Game tab, set them the way you want, and save.
2. **Check your scoreboard messages.** If you customised any leaderboard message, it was
   being ignored and will now start appearing in chat.
3. **`messages.json` migrates itself.** Your customised messages are read from it as
   before, folded into `configs.json` on your next save in the GUI, and the old file is
   then removed. No manual step, no data loss.
4. **Existing `Trivia-log.txt` is kept.** It still shows in the GUI's log viewer, and nothing is
   ever deleted automatically — old logs stay until you remove them yourself.
5. Leftover `Stopped`, `NotRunning`, `ChannelPointBetMode` and `ChannelPointFixedBet`
   keys in your config are harmless and disappear on the next save.

[7.1.1]: https://github.com/aaskjer/Trivia-Game/releases/tag/v7.1.1
[7.1.0]: https://github.com/aaskjer/Trivia-Game/releases/tag/v7.1.0
[7.0.0]: https://github.com/aaskjer/Trivia-Game/releases/tag/v7.0.0
[6.1.0]: https://github.com/aaskjer/Trivia-Game/releases/tag/v6.1.0
