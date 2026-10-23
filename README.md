<p align="center">
  <img src="https://github.com/aaskjer/Trivia-Game/blob/main/Assets/TG/Trivia-Game.png?raw=true" alt="Trivia Game" width="340" height="340">
</p>

<p align="center">
  <b>Quiz rounds straight in your chat — on Twitch, YouTube and Kick.</b><br>
  Questions come from the Open Trivia Database, from a list you write yourself, or both.
</p>

<p align="center">
  <img src="https://img.shields.io/badge/platforms-Twitch%20%7C%20YouTube%20%7C%20Kick-9146FF" alt="Platforms">
  <img src="https://img.shields.io/badge/tool-streamer.bot-0b73ff" alt="Streamer.bot">
  <img src="https://img.shields.io/github/license/aaskjer/Trivia-Game" alt="License">
  <img src="https://img.shields.io/github/v/release/aaskjer/Trivia-Game" alt="Release">
  <img src="https://img.shields.io/github/downloads/aaskjer/Trivia-Game/total" alt="Downloads">
</p>

<p align="center">
  <a href="Setup-Guide.md"><b>Setup Guide</b></a> &nbsp;·&nbsp;
  <a href="FAQ.md"><b>FAQ</b></a> &nbsp;·&nbsp;
  <a href="Import-String.md"><b>Import String</b></a> &nbsp;·&nbsp;
  <a href="https://github.com/aaskjer/Trivia-Game/releases"><b>Download</b></a>
</p>

---

## A round in chat

This is the shape of a round as it ships — placeholders and all. All 63 chat lines are yours to
rewrite on the Messages page.

```
❓
{question}
▶ 1. {answer}
▶ 2. {answer}
▶ 3. {answer}
▶ 4. {answer}
| 🌟 {difficulty} | 📚 {category} | ⏳ {seconds}s to answer!

🎉 Winners: {winners}! The correct answer was: ‘{correctAnswer}’. 🏆
🏅 {winners} each earned {points} {pointsName}!
```

In a normal round everyone who got it right wins. In a **blitz** round the first correct answer
ends it on the spot, on a shorter timer and for a bigger payout.

---

## What it does

- **Every platform plays its own round.** Twitch, YouTube and Kick each keep a separate game, and
  the reply goes back to the chat the command came from.
- **Four ways to answer.** By option number, by exact text, normalized, or fuzzily by Levenshtein
  distance — you decide which of those count.
- **Your questions or theirs.** Draw from the Open Trivia Database, from your own list, or mix the
  two. Viewers can write trivia for you with `!Suggest`.
- **Payouts that scale.** Difficulty and blitz rounds raise the reward. Subscribers and VIPs can be
  boosted, viewers on the addicted list docked.
- **Rounds that start themselves.** A Streamer.bot timed action can open a round on its own, and
  one command or redemption can play several back to back.
- **A board and stats.** A leaderboard, personal stats, stats for any viewer, with an optional
  monthly reset per platform. Wins, points and blitz stats live in Streamer.bot user variables.
- **One settings window for all of it.** Game rules, questions, points, channel points, every chat
  message and the action log.

---

## Commands

These are the default names — in Streamer.bot you can bind them to whatever you like.

| Command | What it does |
|---|---|
| `!Trivia` | Starts a round. Mod-only while **Require Mod To Start** is on, and replaced by a redemption while **Only Channel Points** is on. |
| `!Trivia <category>` | A named category: `!Trivia history`, `!Trivia video games`. Short forms work too — tv, movies, anime, maths. Needs **Let Chat Pick The Category**. |
| `!Trivia <n> <category>` | Both at once: `!Trivia 3 science` plays three science rounds, and the category sticks for the whole run. |
| *just type in chat* | Your answer. |
| `!SkipTrivia` | Throws the question away without scoring it. The answer stays hidden. |
| `!ForceEndTrivia` | Ends the round early but still scores it — answers already in count, and the answer is revealed. Stops a back-to-back series too, even between two questions. Always mod-only. |
| `!Suggest q \| correct \| wrong \| wrong` | Adds a question of the viewer's own to the pool. Set the guard under **Questions → Suggestions**. |
| `!TA add\|remove <user>` | Adds or removes a viewer from the half-payout list, straight into the Points page. |
| `!TriviaScoreboard` | The top players for the platform the command came from. |
| `!TStats` · `!TSS [@user]` | Your own stats, or somebody else's if you tag them. |

**How an answer is matched.** Each one is checked in a fixed order and the first rule that matches
wins: exact text, then the option number, then normalized text, then a close spelling within the
fuzzy tolerance. The option number is skipped when an option is itself that number, so a question
whose answers are numbers still behaves. Only a viewer's first answer counts.

---

## 🎫 Channel points (Twitch only)

With **Only Channel Points** on, the start command stops working — a round begins when someone
redeems the reward you configured. Everyone can still answer. Points decide *whether* a round may
start and nothing else: nothing is wagered, deducted or paid out in them.

| Option | Info |
|---|---|
| **Reward ID (Single Round)** | The reward that plays exactly one round |
| **Reward ID (Multi Round)** | An optional second reward that plays several, up to **Max Rounds Per Redeem** |
| **Refund Failed Redemptions** | Hands the points back when the redemption cannot start a round |

The `Trivia Game` action needs a **Twitch → Channel Point Reward Redemption** trigger for any of
this to reach the script. The current import ships with it; an install that updated only the code
may not have it.



More information and the walk-through are in the [Setup Guide](Setup-Guide.md).

---

## Credits & links

[aaskjer on Twitch](https://twitch.tv/aaskjer) · [aaskjer.github.io](https://aaskjer.github.io/) · [Streamer.bot](https://streamer.bot) ·
[nate1280](https://www.patreon.com/c/nate1280/home) · [Open Trivia Database](https://opentdb.com)

YOU ♡
