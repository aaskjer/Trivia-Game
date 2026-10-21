<p align="center"><img src="https://github.com/aaskjer/Trivia-Game/blob/main/Assets/TG/Trivia-Game.png?raw=true" alt="TG banner" width="400" height="400"></p>

Trivia Game runs quiz rounds straight in your chat, on Twitch, YouTube and Kick. Questions come from the Open Trivia Database.


![Static Badge](https://img.shields.io/badge/https%3A%2F%2Fimg.shields.io%2Fbadge%2Fany_text-Twitch%2C_Kick%2C_YouTube-white?label=supported%20plattform)
![Static Badge](https://img.shields.io/badge/https%3A%2F%2Fimg.shields.io%2Fbadge%2Fany_text-streamer.bot-white?label=tool&color=%230b73ff)
![GitHub License](https://img.shields.io/github/license/aaskjer/Trivia-Game)
![GitHub Release](https://img.shields.io/github/v/release/aaskjer/Trivia-Game)
![GitHub Downloads (all assets, all releases)](https://img.shields.io/github/downloads/aaskjer/Trivia-Game/total)

---

## Overview

- Every platform keeps its own round, normal or blitz. Wins, points and blitz stats are stored in Streamer.bot user variables.
- Difficulty, question type, category, the answer timer and the countdown reminders are all yours to set.
- Answers are matched four ways: by option number, exactly, normalized, or fuzzily by Levenshtein distance.
- Payouts scale with difficulty and with blitz rounds. Subscribers and VIPs can be boosted, and viewers on the addicted list can be docked.
- Draw questions from the Open Trivia Database, from a list you write yourself, or mix the two — and let viewers suggest their own from chat.
- A Streamer.bot timed action can start rounds on its own, and one command or redemption can play several back to back.
- A leaderboard, personal stats and stats for any viewer, with an optional monthly reset per platform.
- Everything is configured in a settings window: game rules, questions, points, channel points, every chat message, and the action log.

---

## 🎫 Channel Points Mode (Twitch only)

With **Only Channel Points** switched on, the start command stops working — a round begins only when someone redeems the reward you configured. Everyone can still answer. Channel points decide *whether* a round may start and nothing else: nothing is wagered, deducted or paid out in them.

| Option | Info |
|---------|---------|
| **Reward ID (Single Round)** | The reward that plays exactly one round |
| **Reward ID (Multi Round)** | An optional second reward that plays several rounds, up to **Max Rounds Per Redeem** |
| **Refund Failed Redemptions** | Hands the points back when the redemption cannot start a round |

The `Trivia Game` action needs a **Twitch → Channel Point Reward Redemption** trigger for any of this to reach the script. The current import ships with it; an install that updated only the code may not have it.

---

## Get Started

[Setup Guide](https://github.com/aaskjer/Trivia-Game/blob/main/Setup-Guide.md)

[Frequently Asked Questions](https://github.com/aaskjer/Trivia-Game/blob/main/FAQ.md)

[Import String for streamer.bot](https://github.com/aaskjer/Trivia-Game/blob/main/Import-String.md)

[Download](https://github.com/aaskjer/Trivia-Game/releases)

---

## Credits & Links


[aaskjer on Twitch](https://twitch.tv/aaskjer)

[Streamer.bot](https://streamer.bot) / [nate1280](https://www.patreon.com/c/nate1280/home)

YOU ♡
