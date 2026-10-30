<p align="center">
  <img src="https://github.com/aaskjer/Trivia-Game/blob/main/Assets/TG/Trivia-Game.png?raw=true" alt="Trivia Game" width="340" height="340">
</p>

<h1 align="center">Trivia Game</h1>

<p align="center">
  <b>Quiz rounds straight in your chat — on Twitch, YouTube and Kick.</b><br>
  Questions from the Open Trivia Database, Wikidata, OdeNet, PokeAPI or IGDB — or from a list you write yourself.
</p>

<p align="center">
  <img src="https://img.shields.io/badge/platforms-Twitch%20%7C%20YouTube%20%7C%20Kick-9146FF" alt="Platforms">
  <img src="https://img.shields.io/badge/tool-streamer.bot-0b73ff" alt="Streamer.bot">
  <img src="https://img.shields.io/github/license/aaskjer/Trivia-Game" alt="License">
  <img src="https://img.shields.io/github/v/release/aaskjer/Trivia-Game" alt="Release">
  <img src="https://img.shields.io/github/downloads/aaskjer/Trivia-Game/total" alt="Downloads">
</p>

<p align="center">
  <a href="https://github.com/aaskjer/Trivia-Game/releases"><b>Download</b></a> &nbsp;·&nbsp;
  <a href="Setup-Guide.md"><b>Setup Guide</b></a> &nbsp;·&nbsp;
  <a href="FAQ.md"><b>FAQ</b></a> &nbsp;·&nbsp;
  <a href="Import-String.md"><b>Import String</b></a>
</p>

---

## 📖 Overview

Trivia Game is a quiz for Streamer.bot. A round posts a question with numbered options and a countdown.
In a normal round everyone who answers correctly wins. In a blitz round the first correct answer ends it,
on a shorter timer and for a bigger payout. Twitch, YouTube and Kick each play their own round.

## ✨ Highlights

- 📚 **Six question sources.** The Open Trivia Database, Wikidata, German vocabulary from OdeNet, Pokémon from
  PokeAPI, video games from IGDB, or your own list, alone or mixed with opentdb. Viewers can submit
  questions with `!Suggest`.
- 🌟 **Difficulty that means something.** Wikidata, PokeAPI and IGDB rank what they know by how well known it
  is, so *easy* asks about Japan and Pikachu and *hard* about Nauru and Garbodor.
- ✅ **Lenient answers, strict numbers.** Answers match by option number, exact text, normalized text or a close
  spelling. A number is never "close": 1995 is not 1994. Only a viewer's first answer counts.
- 🔁 **Rounds that start themselves.** A Streamer.bot timed action can open a round, and one command or
  redemption can play several back to back.
- 📺 **An overlay that gives nothing away.** It shows the question, the countdown and how many have answered,
  never what they picked. The answer appears only once the round is over, followed by the winners and the
  leaderboard. It runs on Streamer.bot's own WebSocket server and picks a running round back up after a
  scene switch.
- 💰 **Payouts that scale.** Difficulty and blitz rounds raise the reward. Subscribers and VIPs can be boosted,
  viewers on the addicted list docked. Leaderboard and stats can reset monthly per platform.
- 🎫 **Channel Points as a start button.** On Twitch a redemption can start a round. Nothing is wagered or paid
  out in channel points.

<p align="center">
  <img src="https://github.com/aaskjer/Trivia-Game/blob/main/Assets/overlay/round.png?raw=true" alt="A round running on the overlay" width="49%">
  <img src="https://github.com/aaskjer/Trivia-Game/blob/main/Assets/overlay/result.png?raw=true" alt="The result of that round on the overlay" width="49%">
</p>

---

## Credits

- **Author:** [aaskjer on Twitch](https://twitch.tv/aaskjer) · [aaskjer.github.io](https://aaskjer.github.io/)
- **Built for:** [Streamer.bot](https://streamer.bot) by [nate1280](https://www.patreon.com/c/nate1280/home)
- **Data & services:**
  - [Open Trivia Database](https://opentdb.com): questions
  - [Wikidata](https://www.wikidata.org) (CC0): generated questions
  - [OdeNet](https://github.com/hdaSprachtechnologie/odenet) by Melanie Siegel & Francis Bond (CC BY-SA 4.0): German vocabulary
  - [PokeAPI](https://pokeapi.co): Pokémon
  - [IGDB.com](https://www.igdb.com): video games
- **Thanks:** YOU ♡

Pokémon and its names are trademarks of Nintendo, Creatures and GAME FREAK. None of the projects above is
affiliated with or endorses Trivia Game.
