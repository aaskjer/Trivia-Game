<p align="center"><img src="https://github.com/aaskjer/Trivia-Game/blob/main/Assets/TG/Trivia-Game.png?raw=true" alt="TG banner" width="400" height="400"></p>
Trivia Game is a Streamer.bot chat game for livestream platforms that runs interactive trivia rounds directly in chat, with support for Twitch, YouTube, Trovo, and Kick.
It fetches questions from Open Trivia Database.


# Overview

- Manages active rounds per platform, supports normal and blitz modes, and tracks wins, points, and blitz stats through Streamer.bot user variables.
- Configurable rounds with difficulty, question type, category, answer timers, and countdown reminders.
- Flexible answer matching with numeric, exact, normalized, and fuzzy matching using Levenshtein distance.
- Points system with difficulty multipliers, blitz multipliers, subscriber/VIP boosts, and optional addicted-user penalties.
- Built-in leaderboard, personal stats, target-user stats, and optional monthly resets per platform.
- WPF settings UI for configuration, scoreboard settings, action-log viewing, channel points setup, and message customization.

---

## 🎫 Channel Points Mode (Twitch only)

When `Use Channel Points To Start` is enabled, the start command is effectively disabled — only viewers who redeem the configured Channel Point reward can start a game, but everyone can play. Four modes are available:

| Option | Info |
|---------|---------|
| **Cost** | The reward's point cost is the bet |
| **Input** | The viewer types their bet into the reward's text field |
| **Fixed** | Every redemption bets a fixed configured amount |
| **Free** | All redemptions play for free regardless of cost |

---

# Get Started

[Setup Guide](https://github.com/aaskjer/Trivia-Game/blob/main/Setup-Guide.md)

[Frequently Asked Questions](https://github.com/aaskjer/Trivia-Game/blob/main/FAQ.md)

[Import String for streamer.bot](https://github.com/aaskjer/Trivia-Game/blob/main/Import-String.md)

[Download](https://github.com/aaskjer/Trivia-Game/releases)

---

# Credits & Links


[aaskjer on Twitch](https://twitch.tv/aaskjer)

[Streamer.bot](https://streamer.bot) / [nate1280](https://www.patreon.com/c/nate1280/home)

YOU ♡
