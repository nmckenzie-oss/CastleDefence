# Adding a new question bank (for teachers)

Question bank *files* live in this folder, but the codes students type on the
title screen are mapped to those files directly inside `index.html` — there's
no separate manifest file to keep in sync.

## 1. Create the question file

Make a new `.json` file in this folder (e.g. `92ch-chemistry.json`) with this exact format:

```json
[
  {
    "q": "Question text goes here?",
    "o": ["Option A", "Option B", "Option C", "Option D"],
    "a": 1
  }
]
```

- `"q"` — the question text
- `"o"` — exactly 4 answer options, in any order
- `"a"` — the index (starting at 0) of the correct option in `"o"`. In the example above, `"a": 1` means "Option B" is correct.

Include as many questions as you like (10–40 is a good range).

## 2. Register the code in index.html

Open `index.html` and search for `QUESTION_BANK_FILES` (it's near the top of the
`<script>` section, just after the default question list). Add a new line:

```js
const QUESTION_BANK_FILES = {
    '93bf': { file:'question-banks/93bf-biology.json', name:'Year 9 Biology' },
    '92ps': { file:'question-banks/92ps-physics.json',  name:'Year 9 Physics' },
    '92ch': { file:'question-banks/92ch-chemistry.json', name:'Year 9 Chemistry' }, // <- new
};
```

- The key (`'92ch'`) is the code students/teachers type on the title screen. Codes are lowercase, but the game accepts any case.
- `file` is the path to the JSON file you made in step 1.
- `name` is shown in the HUD while playing.

Suggested code format: `<year><term><subject initial><detail>` — e.g. Year 9
biology, full bank = `93bf`; Year 9 physics, speed topic = `92ps`.

Save `index.html` — that's the only code file that ever needs editing.

## 3. Play

On the title screen, enter the code (e.g. `92ch`) in the "Question Bank Code" box before starting. Leaving it blank uses the built-in general knowledge bank.

## Important: hosting

Because the game loads these files with `fetch()`, it must be run from a web server (not opened directly as a local `file://` in the browser, which most browsers block for security reasons). Easy options:

- Upload the whole folder (index.html + question-banks/) to a school LMS, Google Sites, GitHub Pages, or similar.
- Or run a quick local server, e.g. from a terminal in this folder: `python3 -m http.server 8000`, then open `http://localhost:8000`.

If the game can't reach a question bank, or the code doesn't match anything in `QUESTION_BANK_FILES`, it will show a warning and fall back to general knowledge questions so the game is never unplayable.
