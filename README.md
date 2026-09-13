# 📦 Box Nester

A recursive box-pushing puzzle game, inspired by *Patrick's Parabox* style mechanics — built with plain HTML5 Canvas + JavaScript, no build step, no dependencies. Works on desktop (keyboard) and mobile (touch/swipe + on-screen controls).

**[▶ Play it live](#)** *(enable GitHub Pages on this repo and paste the link here)*

## How to play

- **Move**: Arrow keys / WASD (desktop) or the D-pad / swipe (mobile).
- **Push**: Walk into a box to push it forward one tile.
- **Pull**: Toggle **Pull mode** (Shift key or the on-screen button), then move — any box directly behind you gets pulled along.
- **Merge**: Push or pull a box onto another box to nest it *inside* that box.
- **Enter a box**: Face a box and press **E** (or tap "Enter / Exit") to step inside and see what's nested there.
- **Retrieve**: Some boxes already have another box hidden inside from the start. Push that cargo box onto the dashed tile at the center of the room to send it back out into the outer room, where you can push it around and merge it like any other box.
- **Exit a box**: Stand on the dashed tile and press **E** again (without pushing anything onto it) to pop back out yourself.
- **Goal**: Keep merging until only **one box** remains in the room — every box nested inside a single box.

## Difficulty

All 20 levels now use denser mazes and more walls than a basic Sokoban room, and several (5, 9, 13, 17, 20) start with a box that already has cargo nested inside it — part of the challenge is realizing you need to go in and get it. Every level has been verified solvable with a scripted engine test before shipping.

## Project structure

```
box-nester/
├── index.html      # Page layout + on-screen controls
├── style.css        # Dark theme UI, responsive for mobile & desktop
├── js/
│   ├── levels.js    # 20 levels of increasing difficulty (data only)
│   └── game.js       # Game engine: recursive world model, input, rendering
└── README.md
```

## How the recursion works

Every box has its own small interior "room". When you push box **B** onto box **A**, B is removed from the current room and placed inside A's interior — so A now visually contains B. You can walk into A to see B sitting in there, or you can just keep playing outside; either way, the win condition is simply: **the room you started in has exactly one box left.**

Levels increase in difficulty by combining:
- more boxes to merge,
- dead-end alcoves that require **pulling** a box out,
- walls that require solving merges in a specific **order** to open a path,
- a few levels where you **start inside a box** and must exit before you can solve the room.

## Running locally

No build tools needed — it's static HTML/JS. Just open `index.html` in a browser, or serve the folder:

```bash
npx serve .
# or
python3 -m http.server
```

## Publishing to GitHub Pages

1. Push this folder to a GitHub repo.
2. Go to **Settings → Pages**.
3. Set the source to the `main` branch, root folder.
4. Your game will be live at `https://<username>.github.io/<repo-name>/`.

## Adding your own levels

Open `js/levels.js`. Each level is:

```js
{
  name: "My Level",
  map: [
    "##########",
    "#..P.....#",
    "#..1..2..#",
    "##########",
  ],
  boxColors: { "1": "#3ddc84", "2": "#e91e63" }
}
```

`#` = wall, `.` = floor, `P` = player start, `1`–`9` = boxes (matched to `boxColors`). Add `startInside: "1"` to make the player begin inside that box's interior.

To give a box pre-loaded cargo (for a retrieval puzzle), add `interiors` and `nested`:

```js
interiors: { "1": { size: 7 } },   // bump the room to 7x7 so there's space to maneuver
nested: { "1": [ { color: "#e91e63", x: 2, y: 2 } ] }
```

Keep cargo at least 2 tiles from every wall of its room (e.g. `x`/`y` of 2 or 4 in a size-7 room) — a box placed directly in a corner can never be pushed out again, a classic Sokoban dead end.

## License

MIT — do whatever you like with it.
