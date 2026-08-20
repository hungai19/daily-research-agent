# The Research Prompt

Turn a pile of YouTube videos into a three-section daily brief — insights, the trend, and video ideas.

No install, no API key, no server. It runs in NotebookLM, in about two minutes.

This is the exact prompt behind [Daily Research Agent](https://github.com/hungai19/daily-research-agent),
which has produced a brief every morning for 64 days straight. Not a simplified version of it — the same
text the server pastes at 6:30 every day.

---

## Use it in 4 steps

1. Open [notebooklm.google.com](https://notebooklm.google.com) and create a new notebook.
2. **Add 5–10 YouTube URLs** as sources — the videos you would have watched today. Wait until every source
   is finished processing.
3. Paste the prompt below into the chat.
4. Read the answer. Two minutes, not two hours.

Do this every morning and you have a research habit. Do it for a week and you will want it automated —
that is what the repo is for.

---

## The prompt

```
Analyse the YouTube videos in this notebook.

Reply with exactly the three sections below and nothing else. Do not restate
these instructions. Do not add a preamble or a closing remark.

## INSIGHTS
Exactly three bullets. Each bullet is one line: a bold headline of 3-8 words,
then an em dash, then ONE sentence explaining why it matters to a solo builder
shipping AI automation. No sub-paragraphs.

## TREND
Exactly one sentence naming the single biggest trend across today's videos.

## VIDEO IDEAS
Exactly three bullets. Each bullet is one line: a bold video title, then an em
dash, then one sentence naming a concrete pain point the video would solve.

Stay under 500 words total. Do not repeat information between sections.
```

---

## Make it yours — change one line

Find this line:

> *"...why it matters to **a solo builder shipping AI automation**."*

Replace that phrase with who you actually are. That single phrase decides what the model considers
worth reporting.

| You are | Write |
|---|---|
| An e-commerce owner | `an e-commerce owner running a Shopify store alone` |
| A freelance video editor | `a freelance editor who bills by the project` |
| A developer at a startup | `a backend developer at a 10-person startup` |

Change the last section too if you are not making videos — `## VIDEO IDEAS` works just as well as
`## THINGS TO TRY THIS WEEK` or `## CLIENT ANGLES`.

**It really does change the answer.** Same six videos, one phrase swapped:

| `a solo builder shipping AI automation` | `an e-commerce owner running a Shopify store alone` |
|---|---|
| *Mastering Context Engineering Over Simple Prompts* | *Plug-and-Play Connectors Recover Lost Sales* |
| *Leveraging Agent Modes for Batch Production* | *Automated Batch Content Speeds Up Marketing* |
| idea: *Connect Claude Directly to Your Business Apps* | idea: *How to Fix Your Store's €3,000 Email Marketing Leak* |

**Notice what this prompt does *not* have: a `[YOUR ROLE]` placeholder.** That is on purpose — see below.

---

## How to tell the answer is broken

The model will almost never say "I failed". It will hand you something that looks right. These are the four
checks the automated version runs on every single answer before it is allowed to send anything:

| Check | What it catches |
|---|---|
| **Is it empty?** | A blank answer that still counts as "success" |
| **Does it start with an error string?** | The tool returned an error message and the pipeline treated it as content |
| **Is it under ~200 characters?** | The model gave up but stayed polite about it |
| **Are all three headings there?** | The answer drifted off format and the sections you rely on are gone |

If an answer fails any of these: **do not fix it by rewriting the prompt.** Check the sources first.
Nine times out of ten a source failed to process and the model was answering about four videos when
you thought it had nine.

### Two things you will notice, and neither is a bug

**Citation markers like `[1-3]`.** Leave them. In NotebookLM they are clickable — that is how you check
a claim against the actual video instead of trusting it. The automated version strips them out, but only
because Telegram cannot click a link into a source.

**The chat remembers your last answer.** Ask twice in the same notebook and the second answer leans on
the first — I changed one word to `these videos` and the reply still came back saying `today's videos`,
echoing the previous turn. If you want a clean read, **start a new notebook for each batch**. That is also
why the automated version keeps one notebook per week rather than one forever.

---

## The mistake that cost me a week

The first version of this prompt used `**Title** —` to show the shape I wanted each bullet to take.

The model took it literally. Every bullet came back starting with the word "Title", and the instructions
themselves were echoed back inside the answer. The pipeline reported success every morning. The output
was garbage every morning.

Two rules came out of that, and both are in the prompt above:

1. **A placeholder has to be unmistakably a placeholder — or better, don't use one at all.** That is why
   this prompt ships with a real working phrase instead of `[YOUR ROLE]`. You edit a working sentence
   rather than fill in a blank the model might read as content.
2. **"Do not restate these instructions" has to be said out loud.** It is not implied.

---

## Want it to run without you?

[github.com/hungai19/daily-research-agent](https://github.com/hungai19/daily-research-agent) —
reads RSS from the channels you choose, filters out Shorts, loads what is left into NotebookLM,
runs this exact prompt, and delivers the brief to Telegram at 6:30 every morning.

The video explains the six decisions that keep it alive, and the four times it broke.
