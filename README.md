# Learn the Vibe

**Let the AI write the code. Understand every line of it.**

Learn the Vibe is a skill for [Claude Code](https://code.claude.com/docs/en/overview). It turns Claude into a kind teacher who is also a senior developer. Whenever Claude writes, changes or explains code, it also teaches it: the ideas first, then the code shown piece by piece in the chat, then a short quiz.

It is made for people who build with AI and want to learn programming at the same time, instead of ending up with a project they cannot explain.

## What you get

| Part | What Claude does |
| --- | --- |
| **Friendly, very simple voice** | Small, common English words and short sentences, one idea each. Every technical word is explained the moment it appears. "Why" comes before "how". |
| **Concept cards** | Every framework, library, tool, language feature or pattern the code uses gets a small card in a quote block: what it is, why it exists, how it is used here, and what to watch out for. |
| **Engineer's view cards** | Short cards on system design and software engineering: where a piece sits in the system, the trade-off behind a choice, the principle at work, and what happens with more data, more users or a failure. |
| **Flow diagrams** | A small text diagram of the pieces and what moves between them, with a ⭐ on the piece being worked on. |
| **Lesson sizes** | `quick` for tiny changes, normal by default, `deep` for big ones. With no argument, Claude picks the size from the size of the work. |
| **Predict, then look** | Once per lesson, before showing the key piece, Claude asks you to guess one thing about it. Guessing first makes the explanation stick. |
| **Memory across sessions** | A notes file records which concepts you have met and how you did on them, so known concepts are not explained again and tricky ones come back a few days later. |
| **Personal glossary** | Every card is saved to one file you can re-read or search. |
| **Review mode** | `/learn-the-vibe review` quizzes you on past concepts with no coding task. |
| **Something to read first** | Before Claude touches any file, it sends the todo list and a short explanation, so you have something to read while it works. |
| **Code in the chat** | Each function, class, type or important line is shown as a real snippet with a link to the file, teaching comments marked `👉`, and a plain explanation underneath. |
| **Wrap-up** | A 4–7 sentence summary, the list of files added or changed, and which tests or checks ran. |
| **Quiz** | Three multiple-choice questions you answer by clicking: one about the code, one about a concept, one about design. Claude then sends the correct answers and why. |

## What it looks like

A concept card, placed just before the code that uses the concept:

> **📘 useMemo** (React)
> **What it is:** a way to tell React "remember this result, and only work it out again when these values change".
> **Why it exists:** a component re-runs every time it re-renders. Without it, slow work is repeated on every render.
> **Here:** we use it so the price list is sorted once, not on every keystroke.
> **Watch out:** if you forget a value in the list at the end, you get an old, wrong result.

An Engineer's view card, for the design side:

> **🏗️ Engineer's view: where the filter lives**
> **The choice:** we filter the products on the server, not in the browser.
> **Why:** the list can grow to many thousands of rows. Sending all of them to the browser would be slow.
> **The other way:** filtering in the browser is simpler and feels instant, and is fine for a small list.
> **The cost:** each filter change now needs a request to the server.
> **The principle:** do the heavy work close to the data.

A piece of code, as it is shown in the chat:

```tsx
// 👉 useMemo wraps the sorting work so React can remember the result.
const sortedPrices = useMemo(
  // 👉 [...prices] makes a copy first, because sort() changes the list it is called on.
  () => [...prices].sort((a, b) => a.amount - b.amount),
  // 👉 "Only redo this when `prices` changes." This list is called the dependency array.
  [prices],
);
```

The `👉` comments exist only in the chat. The file itself keeps the comments your project would normally have, so teaching notes never end up in code your team reviews.

The quiz at the end:

```text
1. If we removed [prices] from the end of useMemo, what would happen?
   ○ The list would be sorted again on every render
   ○ The list would never update when prices change
   ○ React would show an error and stop

2. What is a "hook" in React?
   ○ A function that lets a component use React features such as state
   ○ A special kind of HTML tag
   ○ A file where styles are kept

3. Why do we copy the list with [...prices] before sorting?
   ○ Copying makes sorting faster
   ○ sort() changes the original list, and we must leave `prices` untouched
   ○ useMemo only accepts copied lists
```

## Install

You need Claude Code. The skill is one Markdown file with no scripts and no dependencies.

Clone the repository, then link the skill into your personal skills folder so it works in every project:

```bash
git clone https://github.com/Rghaf/learn-the-vibe.git ~/learn-the-vibe
```

```bash
mkdir -p ~/.claude/skills && ln -s ~/learn-the-vibe/skills/learn-the-vibe ~/.claude/skills/learn-the-vibe
```

Start a new Claude Code session. The skill appears as `/learn-the-vibe`.

To use it in a single project only, link or copy the folder into that project's `.claude/skills/` instead.

## Use it

**On demand.** Type `/learn-the-vibe` followed by what you want:

```text
/learn-the-vibe add a search box to the products page
```

```text
/learn-the-vibe explain how src/auth/login.ts works
```

**Pick a size.** Put `quick` or `deep` first to set how big the lesson is:

```text
/learn-the-vibe quick rename the price column
```

```text
/learn-the-vibe deep add CSV export to the orders page
```

| Size | For | The lesson |
| --- | --- | --- |
| `quick` | A typo, a rename, a one-line fix | Two sentences, the changed lines, at most one card. No todo list, no quiz. |
| normal (default) | Most work | The full flow below, with a three-question quiz. |
| `deep` | A new feature or a new part of the system | The full flow, every piece shown, a design card for each choice, a four-question quiz. |

**Review.** Revise what you have learned, with no coding task. Add a topic to narrow it:

```text
/learn-the-vibe review
```

```text
/learn-the-vibe review React
```

Claude picks up to eight concepts, starting with the ones you got wrong before, and asks them in rounds of four.

**Automatically.** Claude loads the skill on its own when you ask it to write, change or explain code. For plain questions that involve no code, it stays out of the way.

**Always on.** To make Claude use it in every session without being asked, add this to `~/.claude/CLAUDE.md`:

```markdown
# Always-on skills
- **learn-the-vibe** (`~/.claude/skills/learn-the-vibe/SKILL.md`) — load it whenever writing, changing or explaining code.
```

## How a session flows

1. **Before the work: something to read.** Sent before any file is edited: what we are building and why, the todo list, concept cards for the main ideas, an Engineer's view card with a flow diagram for the big picture, the mistakes people commonly make, and the shape of the solution in plain words. It takes a minute or two to read while Claude works.
2. **The work.** Claude writes the code into your files in the project's own style and runs the checks.
3. **Predict, then look.** Claude asks you to guess one thing about the key piece and waits for your reply. A rough guess is perfect.
4. **The code: piece by piece.** For each piece, it names it with a clickable link, shows the real snippet with `👉` comments, explains what goes in and what comes out, and adds a card for any new concept. The todo list is shown again with finished steps ticked.
5. **After the code: wrap-up.** Summary with the main design choice and its trade-off, files changed, what was tested, and a fresh card for any concept that is due to come back.
6. **Quiz.** Three questions with clickable options. After you answer, Claude sends ✅ or ❌ for each, the correct answer, and a sentence or two on why. If you picked a wrong option, it also says why that option was tempting.

When you only ask for an explanation of existing code, step 2 is skipped and Claude teaches the code that is already there.

## Your notes and glossary

Claude keeps two files in `~/.claude/learn-the-vibe/`. They are shared by all your projects and are not part of this repository.

**`notes.md`** has one line per concept:

```markdown
- useMemo (React) | new | carded 2026-10-09
- Zod schema (Zod) | shaky | carded 2026-10-09 | wrong 2026-10-09 | review from 2026-10-12
- Promise (JavaScript) | known | carded 2026-10-02 | right 2026-10-09
```

| Status | Meaning | Next time the concept comes up |
| --- | --- | --- |
| `new` | You saw the card but were never quizzed on it | A one-line reminder |
| `shaky` | Your last quiz answer was wrong | The full card again, in fresh words, and a quiz question once three days have passed |
| `known` | Your last quiz answer was right | Nothing; Claude just uses the name |

**`glossary.md`** is your own book of cards: one heading per concept in A–Z order, with the full card, the date and the project it came from.

Both are plain Markdown. Edit them freely: mark something `known` to stop hearing about it, or delete a line to see its card again.

## Design choices

- **Teaching comments stay in the chat.** Snippets in the chat carry extra `👉` comments; the files do not.
- **Snippets are copied from the file after writing.** What you read in the chat is the code that exists on disk, not a retyped version.
- **One idea per snippet.** About 5–25 lines. Long functions are cut into parts and walked through in order.
- **Concepts are not repeated in full.** A concept you have already met gets a one-line reminder, or nothing once you have answered a quiz question on it correctly.
- **Wrong answers come back.** A concept you got wrong returns as a fresh card and a quiz question three days later.
- **It answers in your language.** Write in Italian, Persian or anything else and the teaching follows.
- **It leaves design decisions to you when a learning plugin asks for them.** With [VibeWise](https://github.com/nykooi1/vibe-wise) active, it teaches the concepts first and holds back the solution outline until VibeWise's implementation step.

## Works well with

- [VibeWise](https://github.com/nykooi1/vibe-wise): you reason through the design before Claude writes code. Learn the Vibe then shows and explains the code VibeWise's reports leave out.
- [ponytail](https://github.com/DietrichGebert/ponytail): keeps the code itself as small as possible. Ponytail decides how much code is written; Learn the Vibe decides how it is explained.

## Make it yours

Almost everything lives in [`skills/learn-the-vibe/SKILL.md`](skills/learn-the-vibe/SKILL.md). Common changes:

| You want | Change |
| --- | --- |
| A different level (for example intermediate) | The first paragraph and the **Voice** section |
| More or fewer quiz questions | "three" in the **Quiz** section |
| A different gap before a wrong answer comes back | "3 days" in the **Learner notes** section |
| Different rules for what counts as `quick` or `deep` | The table under **Pick the lesson size** |
| A different review session | `review.md` |
| Teaching comments written into the files too | Step 2 of **The code: piece by piece** |
| A full concept card every time, even when repeated | The second bullet under **Concept cards** |
| Different fields on the concept card | The example card under **Concept cards** |

If you installed with the symlink above, edits take effect in the next Claude Code session.

## Repository layout

```text
learn-the-vibe/
├── README.md
└── skills/
    └── learn-the-vibe/
        ├── SKILL.md      the skill
        └── review.md     review mode, loaded only for /learn-the-vibe review
```

## Good to know

- Replies are longer and sessions use more tokens, because Claude explains as it goes. Use `quick` for small things, or say "skip the explanation this time".
- The predict question pauses the lesson until you reply. The code is already written by then, so no work is waiting on your answer.
- The clickable quiz needs a Claude Code surface that supports question pickers. Where it is not available, the questions are written in the chat with lettered options.
- The picker always adds an "Other" choice for typing your own answer. That comes from Claude Code itself.
