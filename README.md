# Interactive Transcript Kit

Make a "Broken Chat"-style teaching exercise: a **real** AI chat session that students read with checkpoint questions embedded in it. One self-contained web page. No server, no account, nothing recorded.

Students download the file, double-click it, and read a numbered transcript. A few turns carry a small dot. Each dot has a multiple-choice checkpoint beside it. Students commit to an answer, then see the instructor's reading of that moment and why each other option is wrong. Their first pick is remembered in their own browser and nowhere else.

Built for a graduate course on law and AI in 2026, and packaged so that anyone with a text editor and an AI assistant such as Gemini or Microsoft Copilot can make their own.

## What is in this repository

| File | What it is |
|---|---|
| `interactive-transcript.html` | The shell. Opens in any browser as a short sample exercise. You replace one block of text inside it and never touch the rest. |
| `example - Ask It Again.json` | A complete, real exercise's content in the exact format the shell expects. Give this to your AI assistant as the model to imitate. |
| `AI assistant instructions.txt` | Paste into a Gemini Gem or a Copilot agent (or as the first message of a chat). It interviews you, then produces the content block. |
| `README.md` | This guide. |

## What you need before you start

- **A real transcript.** The exercise's credibility rests on "this actually happened." Export or copy the whole session, including any source links, attachment names, and system notices. Students need that evidence on the page.
- **Your objectives and boundaries.** What should students be able to do afterwards, and what is deliberately out of scope this week?
- **Your own diagnosis.** What went wrong, in your words. What did you expect at each bad moment? Which of your own moves were mistakes? Was there a moment where the tool was actually fine?
- **Your course's names for failures,** if it has them. The checkpoint feedback teaches that vocabulary.

## The workflow

1. **Set up the assistant.** In Gemini: create a Gem and paste the whole of `AI assistant instructions.txt` as its instructions; attach the example JSON as a knowledge file. In Copilot: create an agent the same way, or start a new chat and paste the instructions as your first message, then attach the example JSON. Attach your transcript too.
2. **Do the interview.** The assistant asks about the transcript, your audience, your diagnosis, and logistics, one topic at a time. Answer in your own words. Your account of any moment beats the assistant's. When it summarizes, confirm, and tell it to build.
3. **Collect the content block.** The assistant returns one JSON object in a code block. If it delivers it in numbered parts, join them in the order it says, in a plain-text editor.
4. **Paste it into the shell.** Open `interactive-transcript.html` in a plain-text editor (Notepad, TextEdit in plain-text mode, VS Code; not Word). Find the two lines marked `CONTENT BLOCK` near the top. Replace everything between them with your JSON. Save.
5. **Open the file in a browser** (double-click it). A **red box** means the JSON has a formatting slip; the box says roughly where. A **yellow "Author check" box** lists schema problems (a marked turn with no checkpoint, a wrong option missing its feedback, and so on). When both are gone, the page is working.
6. **Do the two human gates the design depends on.** First, read every checkpoint reveal yourself and change anything that does not match your reading of the turn. Second, have a colleague check that every wrong option is checkably wrong from the transcript alone. If you disagree about one, rewrite it. Edit the JSON, save, reload.
7. **Try it as a student.** Answer a checkpoint, scroll past the feedback, reload the page (your answer should still be there), and look at it in a narrow window or on a phone.
8. **Post the file** in your course site as a download beside the assignment.

## Gemini versus Copilot

- **Gemini** handles the whole job in one reply and can show the file in Canvas. Use a Gem so the instructions persist across sessions.
- **Microsoft 365 Copilot** has a shorter reply limit. Expect the content block in parts; the instructions tell it how. Ask for "Part 1" (everything except the transcript turns), then the turns three or four at a time, and join them yourself.
- **Either way,** the assistant cannot verify quotations or citations in the transcript against the real sources. It is told to list what it did not check. Check those yourself before you tell students anything was verified.

## Rules that make the exercise work

These were learned by watching real students and instructors get confused. The assistant is told all of them; you are the one who enforces them.

- **One right answer, every wrong option checkably wrong.** A wrong option must be refutable by pointing at a specific line in, or missing from, the transcript. A partly-true option is not a wrong option; its true part belongs in the feedback.
- **Mark fewer than half the problems.** Four to six marks. The unmarked ones are your debrief material and the transfer test.
- **Mark user turns too.** The driver's mistakes are half the curriculum. Include one moment where the honest answer is "nothing broke; check before you trust," if you have one.
- **Keep it real and say how you edited it.** Shortened passages are marked on the page. Nothing is added that the tool did not produce. The user's real prompts stay word-for-word where their wording is the lesson.
- **Plain language throughout.** Define every term at first use, including "checkpoint" and "turn." Say early that it is not graded, not turned in, and records nothing.

## The content block

Everything the page shows comes from one JSON object inside the HTML file:

| Field | What it holds |
|---|---|
| `title`, `subtitle` | Page title and the line under it (plain text) |
| `welcome` | One paragraph stating the whole task (HTML) |
| `steps` | Three "How this works" steps (HTML strings) |
| `practicalNotes` | Not graded, not collected, time estimate (HTML) |
| `scenarioKicker`, `scenario` | The real story, terms defined, editing register disclosed (HTML) |
| `standingNote` | The three-item "before you begin" note (HTML) |
| `quickReference` | Optional drawer: `{ "summary": "...", "html": "..." }` |
| `closing` | `{ "heading": "...", "html": "..." }` |
| `checkpoints` | Keyed by turn number as a string: `{ "locator", "stem", "correct", "options": [{ "text", "fb" }], "answer", "nudge" }` |
| `items` | The transcript in order: `{ "type": "turn", "turn", "speaker": "user" or "assistant", "marked", "html" }` or `{ "type": "system", "text" }` |
| `storageKey` | Optional. Browser-storage key; defaults to the title |

Inside every string, write `</` as `<\/`. That is the one rule the browser will not forgive.

## Troubleshooting

- **Red box: formatting error.** Usual causes: a straight double quote inside text that was not written as `\"`; a missing comma between entries; an extra comma after the last entry in a list; a `</` that was not written as `<\/`.
- **Page is blank with no box.** The `CONTENT BLOCK` markers were damaged, or the file was saved by Word. Re-copy the shell and paste again in a plain-text editor.
- **Old answers appear on a new version.** The browser remembers picks per exercise title. Change the title, or add `"storageKey": "anything-unique"`.
- **The checkpoint does not sit beside its turn.** By design on narrow screens; widen the window past about 1080 pixels.
- **The assistant rewrote the tool's text.** Tell it to restore the original word-for-word. Only shortening is allowed, and it must be marked.

## Attribution

Shell, design rules, and the example: Rebecca Fordon, 2026, developed with Claude. The interaction design (commit-then-explore checkpoints, the novice rule, marking under half the failures) came out of instructor playtesting for the original "Broken Chat" exercise.

License: not yet chosen.
