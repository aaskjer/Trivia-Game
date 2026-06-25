# ❓ Trivia Game — FAQ

***

## 🛠️ Setup & Configuration

**Q: Where are the config files stored?**

All config files live under your Streamer.bot base directory:
```
Chat Games/Trivia Game/configs.json        ← game settings
Chat Games/Trivia Game/messages.json       ← all chat messages
Chat Games/Logs/Trivia-log.txt             ← action log
```
You should never need to edit these by hand — use the built-in Settings GUI instead.

***

**Q: The Settings GUI won't open / crashes immediately. What do I do?**

- Check the Streamer.bot log for error details and please report it on the [issues page](https://github.com/aaskjer/Trivia-Game/issues).
- If `configs.json` or `messages.json` is malformed, the GUI will fail to open. Delete the corrupt file and restart — it will be recreated with defaults.

***

**Q: How do I reset all settings to defaults?**

Open the Settings GUI and click **Reset to Defaults** (red button, bottom-left). This rewrites both `configs.json` and `messages.json` with factory values and reopens the GUI.

***

**Q: I saved settings but nothing changed in-game. Why?**

Clicking **Save** writes the JSON files to disk. The game script reads config fresh on every `Execute()` call, so the next game command will pick up your changes immediately. No Streamer.bot restart needed.

***

## 🎮 Starting & Stopping the Game

**Q: Only moderators can start the game — how do I allow everyone?**

In the Settings GUI → **Game** tab, uncheck **Require Mod To Start**. Broadcasters can always start regardless of this setting.

***

**Q: How do I start with Channel Points instead of a chat command?**

1. Enable **Allow Channel Points To Start** in the Settings GUI → **Other Modules → Twitch Channel Points**.
2. Paste your **Reward ID** into the field.
3. Set your preferred **Bet Mode** (`fixed`, `cost`, `input`, or `free`).

Note: Channel Points only controls *starting* — players can still answer normally using the configured answer command regardless.

***

**Q: What does each Channel Points Bet Mode do?**

| Mode | Behaviour |
|------|-----------|
| `fixed` | Every redemption bets the exact **Fixed Bet Amount** set in the GUI |
| `cost` | Bet equals the reward's **channel point cost** |
| `input` | User types their bet into the reward text field (`rawInput`) |
| `free` | Bet is forced to 0 — free play, no points wagered |

***

**Q: A round is stuck as active after Streamer.bot restarted. How do I clear it?**

Use the mod command `!ForceEndTrivia` (bound to the Stop action). The game state lives in memory, so a Streamer.bot restart already clears it — just start a new round normally.

***

## 💬 Commands & Answering

**Q: What are the in-chat commands?**

| Command | Who | What it does |
|---------|-----|--------------|
| `!StartTrivia` | Mod / Broadcaster / Everyone | Start a new round |
| `!ForceEndTrivia` | Mod / Broadcaster | Stop the current round |
| `!SkipTrivia` | Mod / Broadcaster / Everyone | Skip the current question |
| `!TriviaPersonalStats` | Everyone | View personal or target user stats |
| `!TriviaScoreboardSearch` | Everyone | View target user stats |
| `!TriviaScoreboard` | Everyone | Show the top-N leaderboard |
| `!TriviaAddict add <user>` | Mod / Broadcaster | Add a user to the addicted list |
| `!TriviaAddict remove <user>` | Mod / Broadcaster | Remove a user from the addicted list |

Command names depend on what you configured in Streamer.bot — the above are the defaults.

***

**Q: How do players submit answers?**

Players type their answer directly into chat. The game checks every chat message during an active round against the correct answer using the enabled matching modes. No command prefix is needed for answers.

***

**Q: Can the same answer be submitted twice by the same user?**

No. If a user submits a second answer, the game sends the `AlreadyAnswered` message and ignores it — no penalty is applied.

***

**Q: What is the difference between a normal round and a Blitz round?**

| Round type | Behaviour |
|------------|-----------|
| Normal | All correct answers are collected until time runs out; all winners are announced together |
| Blitz | First correct answer wins immediately and the round ends |

Blitz rounds are triggered randomly based on the **Blitz Chance** percentage configured in the Settings GUI.

***

## 💰 Points & Currency

**Q: What points system does the game use?**

The game auto-detects your currency in this priority order:
1. **StreamUP Currency Core** — reads the `currency_ProductSettings` global.
2. **tawmae Loyalty Points** — reads the `tawmae_Settings_LOYALTY POINTS` global.
3. **Config fallback** — uses `PointsVar` / `PointsName` from `configs.json`.

If you use a custom currency with a different variable name, set **Points Var** and **Points Name** in the Settings GUI → **Game** tab.

***

**Q: How are win points calculated?**

```
awarded = BaseWinPoints × DifficultyMultiplier × BlitzMultiplier × RoleMultiplier
```

`RoleMultiplier` starts at 1.0 and is adjusted by subscriber boost, VIP boost, and the addicted penalty. Blitz multiplier only applies during Blitz rounds.

***

**Q: What difficulty multipliers are available?**

| Difficulty | Default multiplier |
|------------|-------------------|
| Easy | 1.0× |
| Medium | 1.75× |
| Hard | 2.5× |
| Very Hard | 3.0× |
| Blitz bonus | 2.0× (stacks on top) |

All multipliers are configurable in the Settings GUI → **Game** tab.

***

## 👥 Role Management

**Q: How do Subscriber / VIP boosts work?**

Enable the boost in **Settings GUI → Game → Role Boosts** and set the percentage. The multiplier stacks additively:
```
multiplier = 1.0 + (SubBoost%) + (VipBoost%)
```
Example: 10% sub boost + 20% VIP boost on a 1,000-pt win → `1,000 × 1.30 = 1,300 pts`.

***

**Q: What is the "Addicted" system?**

Users on the addicted list receive a **50% point penalty** — they always earn half of what they would normally win. Manage the list with `!addicted add/remove <user>` (mods only) or edit **Addicted Users** directly in the Settings GUI.

***

## 🧠 Questions & Categories

**Q: Where do the trivia questions come from?**

Questions are fetched from the [Open Trivia Database](https://opentdb.com) API. The game supports filtering by **category**, **difficulty**, and **question type** (multiple choice, true/false, or both).

***

**Q: Can I change the default category or difficulty?**

Yes. In the Settings GUI → **Game** tab you can set the **Default Difficulty**, **Default Question Type**, and **Default Category** used for every round.

***

**Q: What happens if the API returns no results for my settings?**

If the API returns a code 1 (no results), the game logs a warning and the round fails to start. Try broadening the category or difficulty range. The game also caches questions in advance to reduce API failures.

***

**Q: How does question caching work?**

On each round start, the game attempts to serve a question from the local cache first. If the cache is empty (cache miss), it fetches a batch of questions from the API, serves one, and stores the rest. The batch size is controlled by **Question Cache Size** in the Settings GUI.

***

## 🛡️ Cooldowns & Moderation

**Q: What is the difference between Global Cooldown and User Cooldown?**

- **Global Cooldown** — applies to *all* users simultaneously after a round starts.
- **User Cooldown** — applies only to the individual user who triggered the start command.

Both are set in the Settings GUI → **General** tab and are applied to the Streamer.bot command directly.

***

**Q: How do I block specific users or groups from playing?**

In the Settings GUI → **General** tab, add usernames to **Blocked Users** and group names to **Blocked Groups** (comma-separated). Blocked users and groups are silently ignored when they send answers.

***

**Q: Can I skip or force-end a round?**

Yes. The `!triviaskip` command skips the current question (and optionally auto-starts the next one if **Auto Next Round After Skip** is enabled). The `!ForceEndTrivia` command immediately ends the running game without awarding points. Both commands require moderator or broadcaster permissions by default.

***

## 🏆 Scoreboard

**Q: How is the leaderboard built?**

The leaderboard reads all user stats stored in Streamer.bot's user variable system and ranks them by total wins, then total points. It shows the top-N users configured in **Scoreboard Top N**.

***

**Q: How do I exclude the broadcaster or bot from the scoreboard?**

In Settings GUI → **Other Modules → Scoreboard**: uncheck **Include Broadcaster** and/or **Include Bot**. You can also add usernames to **Blocked From Board** (comma-separated) to exclude specific accounts such as other bots.

***

**Q: How does the monthly auto-reset work?**

Enable **Reset All (Monthly)** or the per-platform toggles in the Scoreboard settings. On the first scoreboard command of each month (or on/after the configured **Reset Day**), all user stats for that platform are zeroed. A chat announcement is sent and the reset date is saved to a persisted global so it only runs once per month.

***

**Q: Can I view stats for another user?**

Yes. Use `!triviagamestats @Username` to look up another user's wins, points, blitz wins, and blitz points.

***

## 🌐 Multi-Platform

**Q: Which platforms are supported?**

Twitch, YouTube, Kick, and Trovo. Enable the ones you stream on in Settings GUI → **General → Select Platform(s)**. The game determines the active platform automatically from the incoming event.

***

**Q: Can the game run on multiple platforms at the same time?**

Yes. Each platform has its own independent session in memory, so a Twitch round and a YouTube round can be active simultaneously without interfering with each other. Points, stats, and leaderboards are stored and reset per platform independently.

***

## 🔔 Updates

**Q: How do update notifications work?**

On each Settings GUI open, the script calls the GitHub releases API for the latest tag. If a newer version is found, a popup appears offering to open the releases page directly.
