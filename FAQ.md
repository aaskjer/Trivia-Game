# ❓ Trivia Game — FAQ

***

## 🛠️ Setup & Configuration

**Q: Where are the config files stored?**

Everything sits under your Streamer.bot base directory:
```
Chat Games/Trivia Game/configs.json             ← settings and all chat messages
Chat Games/Trivia Game/Trivia-Questions.json    ← the questions you write yourself
Chat Games/Logs/Trivia-log-YYYY-MM-DD.txt       ← action log, a fresh file per day
```
Settings and messages share `configs.json`. An older `messages.json` is still read if it exists and is folded into `configs.json` the next time you save. Log files are kept indefinitely — nothing is deleted automatically.

You should never need to edit these by hand — use the built-in Settings GUI instead.

***

**Q: The Settings GUI won't open / crashes immediately. What do I do?**

- Check the Streamer.bot log for error details and please report it on the [issues page](https://github.com/aaskjer/Trivia-Game/issues).
- A malformed `configs.json` stops the GUI opening at all. Delete the corrupt file and restart — it comes back with defaults.
- If `Trivia-Questions.json` cannot be read, the GUI opens with an empty question list and says so. Fix or move that file before saving, or saving overwrites it.

***

**Q: How do I reset all settings to defaults?**

Open the Settings GUI and click the ↺ button at the bottom left. That rewrites `configs.json` with factory values and reopens the window; the questions you wrote yourself are left alone. For a single page instead, use the ↻ button beside it — that one writes nothing until you save.

***

**Q: I saved settings but nothing changed in-game. Why?**

**Save** writes the JSON files to disk, and the game script re-reads them whenever they change. Your next game command already runs on the new values. No Streamer.bot restart needed.

***

## 🎮 Starting & Stopping the Game

**Q: Only moderators can start the game — how do I allow everyone?**

On the **Access** page, under *Who may run a round*, switch off **Only moderators may start**. Broadcasters can always start, whatever this is set to.

***

**Q: How do I start with Channel Points instead of a chat command?**

1. On the **Access** page, under *Channel points*, tick **Only Channel Points**.
2. Paste your **Reward ID (Single Round)** into the field.

The current import already carries the **Twitch → Channel Point Reward Redemption** trigger on the `Trivia Game` action. On an older install that updated only the code, check **Actions → Trivia Game → Triggers** and add it — without that trigger a redemption never reaches the script at all.

Channel Points only control *starting*. Everyone in chat still answers as usual.

***

**Q: Are channel points wagered or paid out?**

No. They decide *whether* a round may start, and nothing else — nothing is staked, deducted or paid out in channel points, and the reward's cost is purely a matter for Twitch. Winners are paid in your own currency, exactly as they are for a round started from chat.

***

**Q: Can one redemption play several rounds?**

Yes. Put a second, separate reward under **Reward ID (Multi Round)** and a ceiling under **Max Rounds Per Redeem**. Switch on *Require Viewer to Enter Text* on that Twitch reward and viewers type the number themselves, clamped to your ceiling. Leave it off and every redemption simply plays the ceiling.

***

**Q: What happens to the points when a redemption cannot start a round?**

With **Refund Failed Redemptions** on, the redemption is cancelled and the viewer gets their channel points back — whether a round was already running, the command was on cooldown, or no question could be fetched. This needs the reward to keep its redemptions in the request queue, so leave *Skip Reward Requests Queue* switched **off** on Twitch. Redemptions that do start a round are marked fulfilled automatically, so the queue does not pile up.

***

**Q: A round is stuck as active after Streamer.bot restarted. How do I clear it?**

Use the mod command `!ForceEndTrivia`. Game state lives in memory, so a restart has already cleared it — just start a new round normally. A round that outlives its own countdown is caught and resolved on the next chat message, too.

***

## 💬 Commands & Answering

**Q: What are the in-chat commands?**

| Command | Who | What it does |
|---------|-----|--------------|
| `!Trivia` | Mod / Broadcaster / Everyone | Start a new round |
| `!Trivia <n>` | Mod / Broadcaster / Everyone | Play `n` rounds back to back, e.g. `!Trivia 3` |
| `!Trivia <category>` | Mod / Broadcaster / Everyone | Start a round on a named category, e.g. `!Trivia history` |
| `!SkipTrivia` | Mod / Broadcaster / Everyone | Skip the current question |
| `!ForceEndTrivia` / `!FCTrivia` | Mod / Broadcaster | End the round early — answers already given still score |
| `!TStats` | Everyone | Your own stats |
| `!TriviaScoreboardSearch` / `!TSS` | Everyone | Somebody else's stats |
| `!TriviaScoreboard` / `!TTB` | Everyone | Show the top-N leaderboard |
| `!TA` / `!TriviaAddict` `add\|remove <user>` | Mod / Broadcaster | Manage the addicted list |
| `!Suggest q \| correct \| wrong \| wrong` | Reward, group or mod — see *Viewer suggestions* | Add a question of the viewer's own |

Command names depend on what you configured in Streamer.bot — the above are the defaults from the import string.

***

**Q: How do players submit answers?**

They type the answer straight into chat. During a round the game checks every message against the correct answer using whichever matching modes you enabled. Answers need no command prefix.

***

**Q: Can the same answer be submitted twice by the same user?**

No. A second answer gets the `AlreadyAnswered` message and is ignored — no penalty.

***

**Q: What is the difference between a normal round and a Blitz round?**

| Round type | Behaviour |
|------------|-----------|
| Normal | All correct answers are collected until time runs out; all winners are announced together |
| Blitz | First correct answer wins immediately and the round ends |

Which one you get is down to chance: the **Blitz Chance** percentage in the Settings GUI decides how often a round turns into a blitz.

***

## 💰 Points & Currency

**Q: What points system does the game use?**

The game detects your currency itself, in this order:
1. **StreamUP Currency Core** — reads the `currency_ProductSettings` global.
2. **tawmae Loyalty Points** — reads the `tawmae_Settings_LOYALTY POINTS` global.
3. **Config fallback** — uses `VarName` / `PointsName` from `configs.json`.

Running a custom currency under a different variable name? Set **Points variable** and **Shown in chat as** on the **Game** page, under *Points*.

***

**Q: How are win points calculated?**

```
awarded = BaseWinPoints × DifficultyMultiplier × BlitzMultiplier × RoleMultiplier
```

`RoleMultiplier` starts at 1.0 and is adjusted by subscriber boost, VIP boost, and the addicted penalty. The blitz multiplier only applies during Blitz rounds.

***

**Q: What difficulty multipliers are available?**

| Difficulty | Default multiplier |
|------------|-------------------|
| Easy | 1.0× |
| Medium | 1.75× |
| Hard | 2.5× |
| Very Hard | 3.0× |
| Blitz bonus | 2.0× (stacks on top) |

All of them are configurable on the **Game** page, under *Payouts*. opentdb only reports easy, medium and hard — *Very Hard* applies to questions you write yourself.

***

## 👥 Role Management

**Q: How do Subscriber / VIP boosts work?**

Switch the boost on under **Game → Payouts** and set the percentage. Twitch only. The two stack additively:
```
multiplier = 1.0 + (SubBoost%) + (VipBoost%)
```
So a 10% sub boost plus a 20% VIP boost on a 1,000-pt win pays `1,000 × 1.30 = 1,300 pts`.

***

**Q: What is the "Addicted" system?**

Viewers on the addicted list take a **50% point penalty** — they always earn half of what they would otherwise win. Manage the list with `!TA add|remove <user>` (mods only), or edit **Addicted viewers** on the **Access** page. Both write the same list, and a change made from chat while the window is open survives saving.

***

## 🧠 Questions & Categories

**Q: Where do the trivia questions come from?**

From the [Open Trivia Database](https://opentdb.com) API, from a list you write yourself on the **Questions** page, or from both — the **Question source** selector on that page decides. Either source can be filtered by **category**, **difficulty** and **question type** (multiple choice, true/false, or both), and the two sets of filters are kept apart so narrowing one cannot silently empty the other.

Viewers can contribute questions themselves with `!Suggest` once you allow it on the **Questions** page, under *Viewer suggestions*.

***

**Q: Can I change the default category or difficulty?**

Yes. The **Game** page, under *Rounds*, sets the **Difficulty**, **Question type** and **Category ID** used for every round. Turn on **Let chat pick the category** and viewers may name one themselves — `!Trivia history`.

***

**Q: What happens if the API returns no results for my settings?**

Code 1 usually means the batch was simply bigger than the pool your category, difficulty and type leave behind — some opentdb categories hold only a handful of hard questions. The game notices and asks again for a single question, so the round still runs. Only a combination with no questions at all fails, and **Fall Back To The Other Source** can then hand that round to your own list instead. Questions are cached in advance as well, which keeps the API calls down.

***

**Q: How does question caching work?**

Each round start tries the local cache first. On a miss the game fetches a batch from the API, serves one question and keeps the rest. **Fetch ahead** on the **Questions** page sets how big that batch is.

***

## 🛡️ Cooldowns & Moderation

**Q: What is the difference between Global Cooldown and User Cooldown?**

- **Global Cooldown** — applies to *all* users at once after a round starts.
- **User Cooldown** — applies only to the user who triggered the start command.

Both live on the **Access** page under *Cooldown in seconds* and are applied to the Streamer.bot command directly. They gate the command that *starts* a run — the rounds of a back-to-back series run straight through them.

***

**Q: How do I block specific users or groups from playing?**

On the **Access** page, add names under *Blocked viewers* and Streamer.bot groups under *Blocked groups*. Both are ignored silently when they send answers.

***

**Q: Can I skip or force-end a round?**

Yes. `!SkipTrivia` throws the current question away without scoring it or revealing the answer, and optionally starts the next one if **Auto Next Round After Skip** is enabled. `!ForceEndTrivia` ends the round early but still scores it: answers already submitted count and the correct answer is revealed. Force end also stops a back-to-back series, including while it sits between two questions. Both need moderator or broadcaster permissions by default.

***

## 🏆 Scoreboard

**Q: How is the leaderboard built?**

It reads every user stat held in Streamer.bot's user variable system and ranks by total wins, then total points. **Scoreboard Top N** decides how many places it shows.

***

**Q: How do I exclude the broadcaster or bot from the scoreboard?**

Under **Game → Scoreboard**, switch off **Include the broadcaster** and **Include the bot**. For specific accounts such as other bots, add them under **Blocked from the board** — that list applies whatever those two checkboxes say.

***

**Q: How does the monthly auto-reset work?**

Enable **Reset All (Monthly)** or the per-platform toggles in the Scoreboard settings. Switching one on only arms it: the first wipe lands at the next month boundary, never in the middle of the month you enabled it in. After that, the first leaderboard or stats command on or after the configured **Reset Day** zeroes every user stat for that platform, sends a chat announcement, and records the month in a persisted global so it runs only once. A **Reset Day** the month does not have — 31 in February, say — falls on that month's last day instead.

***

**Q: Can I view stats for another user?**

Yes. `!TSS @Username` looks up anyone's wins, points, blitz wins and blitz points.

***

## 🌐 Multi-Platform

**Q: Which platforms are supported?**

Twitch, YouTube and Kick — all three, always. The game works out which one an event came from and answers in that chat, so there is nothing to switch on. To keep it off a platform, restrict the Streamer.bot commands themselves, or take that platform's chat-message trigger off the action.

Channel point rewards are the one Twitch-only part, because no other platform has them.

***

**Q: Can the game run on multiple platforms at the same time?**

Yes. Each platform holds its own session in memory, so a Twitch round and a YouTube round can run at once without touching each other. Points, stats and leaderboards are stored and reset per platform independently.

***

## 🔔 Update Notifications

**Q: How do I know when a new version is available?**

Opening the Settings GUI checks GitHub for the latest release tag. If that tag is genuinely newer than the version you are running, a red **Update** pill appears in the window header — click it to open the releases page. Nothing blocks the window, so the check never costs you the trip.

***

**Q: Can I check for updates without opening the Settings window?**

No — the check runs as part of the `Test` trigger that opens the settings window.

---

# Is Trivia Game an AI Slop?

Partially, yes. This script grew out of input from the Streamer.bot community with AI along for the ride.
But i put a lot of heart and time into it, with the goal of building a chat game that is robust, fair and easy to use for everyone.
I understand that people, especially IT savvy people, will dislike the project because of the use of AI, and i absolutely understand and support their point of view.
Still, i had a lot of fun making it, as with all my other projects — i used it to "learn" coding and put AI to something worthwhile.

AI can create bugs, and i am not a developer in classical terms. But i spend a reasonable amount of time fixing any bug that turns up while testing.
If you still find bugs or have something to say, please let me hear it :)
