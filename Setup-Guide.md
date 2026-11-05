# Import
<p align="center"><img alt="grafik" src="https://github.com/user-attachments/assets/2c17f5a0-15f3-4396-a277-a66638017fb3"> </p>

Click `Import` in [streamer.bot](https://streamer.bot/) and drag the [TriviaGame.sb](https://github.com/aaskjer/Trivia-Game/releases) file into the window, or paste the [import string](https://github.com/aaskjer/Trivia-Game/blob/main/Import-String.md).

<p align="center"><img alt="grafik" src="https://github.com/user-attachments/assets/d3d8ecfd-018d-4052-9056-c72438d5c613"></p>

Click `Import`, then `Yes` on both popups that follow.

## Enable Commands
<p align="center"><img alt="grafik" src="https://github.com/user-attachments/assets/004c325b-8673-430d-91b9-72c9a8931663"></p>

The commands ship disabled. Enable all of them, or the parts that depend on them stay silent.

# Setup
<p align="center"><img alt="grafik" src="https://github.com/user-attachments/assets/8452f85a-daf0-4574-a28c-e9fd0c2eeecf"></p>

Go to `Actions`, select `[TG] - Settings`, right-click the `Test` trigger and choose `Test Trigger`. That opens the settings window.

Eight pages sit on the rail down the left side. `Ctrl+1` to `Ctrl+8` jump between them, `Ctrl+S` saves.

Twitch, YouTube and Kick all play, always — there is nothing to switch on. Restrict the Streamer.bot commands if you want the game off a platform.

## Game
<p align="center"><img alt="grafik" src="https://github.com/aaskjer/Trivia-Game/blob/main/Assets/setup-guide/game.png?raw=true" alt="Game page"></p>

The round and what it pays. Difficulty, question type, category, how long answers are accepted and when the countdown speaks up; then blitz rounds, how closely an answer has to match, rounds that start themselves on a timer, the points a win is worth, and the leaderboard with its monthly reset.

## Questions
<p align="center"><img alt="grafik" src="https://github.com/aaskjer/Trivia-Game/blob/main/Assets/setup-guide/questions.png?raw=true" alt="Questions page"></p>

Three tabs. The first, **Custom List**, holds your own questions, with search and filters — it gets the whole page, because that is where the work happens. The second, **Sources**, decides where each round's question comes from — opentdb, Wikidata, OdeNet, PokeAPI, IGDB, your own list, or yours mixed with opentdb — and in which language, checks that the source answers, and sets which of your own questions may be drawn. The third is whether viewers may suggest questions with `!Suggest`, and under what conditions.

Wikidata, OdeNet and PokeAPI need nothing from you. OdeNet is German only and downloads its word list (about 4 MB) the first time a round needs it. IGDB needs a key of your own:

### IGDB

IGDB belongs to Twitch and only answers an application registered there.

1. Open the [Twitch developer console](https://dev.twitch.tv/console/apps) and click `Register Your Application`.
2. Any name, `http://localhost` as the OAuth Redirect URL, category `Application Integration`, client type `Confidential`. Click `Create`.
3. Click `Manage` on the new application. Copy the **Client ID**, then click `New Secret` and copy the **Client Secret**.
4. In the settings window, pick **IGDB** under **Questions → Sources**, paste both into the fields that appear, and click `Check`. *Twitch accepted the Client ID and Client Secret* means you are done — save.

The secret is stored in `configs.json` on your PC and only ever sent to Twitch, to fetch IGDB's access token. The field hides it, so it stays safe on stream.

## Access
<p align="center"><img alt="grafik" src="https://github.com/aaskjer/Trivia-Game/blob/main/Assets/setup-guide/access.png?raw=true" alt="Access page"></p>

Who gets in and how. A segment switch picks whether rounds start from `!trivia` or from a Twitch channel point reward — Twitch only, since no other platform has rewards, and chat everywhere still answers as usual. Below it: who may start and skip, the two cooldowns, and the blocked and addicted lists as name chips.

## Messages
<p align="center"><img alt="grafik" src="https://github.com/aaskjer/Trivia-Game/blob/main/Assets/setup-guide/messages.png?raw=true" alt="Messages page"></p>

Every line the game says in chat behind one picker, grouped by when it is sent. Pick one and you get its editor, the placeholders it accepts as clickable chips, and a preview of the finished line. The number format and which account speaks sit here too — `/me` is Twitch only, as the pill says.

## Overlay
<p align="center"><img alt="grafik" src="https://github.com/aaskjer/Trivia-Game/blob/main/Assets/setup-guide/overlay.png?raw=true" alt="Overlay page"></p>

The OBS overlay: the running question with its countdown, then the answer, the winners and the leaderboard. It needs Streamer.bot's WebSocket server and nothing else.

1. In Streamer.bot, open `Servers/Clients` → `WebSocket Server`. Tick `Auto Start`, click `Start Server` and note the port — `8080` unless you changed it. If you turn on its authentication, put the password into **Password** on the Overlay page.
2. Connect OBS to Streamer.bot under `Stream Apps` → `OBS Studio`, if it is not already.
3. On the **Overlay** page, click `Set it up in OBS`. It switches on **Send rounds to the overlay**, saves, and adds a browser source called `Trivia Overlay` to the scene `Trivia` — created if needed — at the size of your canvas. Leave **Scene** empty to use the scene that is live in OBS instead. If OBS already has a source of that name, you are asked whether to point it at the overlay again, which is also how a changed port or password reaches OBS.
4. Click `Show a sample round`. One made-up round plays on the overlay in OBS, so you can see where it sits and how it looks. Viewers see it too if you are live.

Without the button: click `Copy URL`, add a `Browser` source in OBS, untick `Local file`, paste the URL into `URL` and set the size to your canvas, usually `1920` × `1080`. `Try the demo` opens the overlay in your browser, playing sample rounds the way the page is set.

**What it shows** switches the answer count, the category and difficulty, the countdown, the winners and the leaderboard on and off on their own, and sets how long a result stays up and how many places the leaderboard lists. **Look** sets where the question sits, its size, the animations and five colours: accent, right answer, blitz, text and panel, with the panel's opacity. A save reaches an overlay that is already open in OBS straight away, and the labels follow the question language.

| Add to the URL | What it does |
|---|---|
| `&platform=twitch` | Only that platform's rounds — `youtube` and `kick` work too. All of them otherwise |
| `&position=top` | The question at the top whatever the Overlay page says, or `bottom` |
| `&lang=de` | German labels whatever the question language, or `en` |
| `&demo=1` | Plays sample rounds without Streamer.bot, to place the source |
| `&debug=1` | A line in the corner that says whether it reached Streamer.bot |

Anything else goes into the source's `Custom CSS` field, which wins over the colours on the Overlay page:

```css
:root { --tg-width: 980px; --tg-radius: 8px; --tg-font: 'Bahnschrift', sans-serif; }
```

`--tg-accent`, `--tg-correct`, `--tg-blitz`, `--tg-text`, `--tg-muted`, `--tg-card`, `--tg-scale` and `--tg-edge` (the gap to the edge of the screen) can be set the same way.

## Log
<p align="center"><img alt="grafik" src="https://github.com/aaskjer/Trivia-Game/blob/main/Assets/setup-guide/log.png?raw=true" alt="Log page"></p>

Every round, answer and payout, searchable by user, platform or outcome. One file per day, and nothing is deleted automatically.
