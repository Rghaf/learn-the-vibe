# Review mode

A revision session with no coding task: quiz the user on what they have already met, so it stays in memory. Use the voice, the learner notes and the quiz rules from [SKILL.md](SKILL.md).

1. **Read** `~/.claude/learn-the-vibe/notes.md` and `glossary.md`. When there are no notes yet, say so kindly, explain that cards are saved as we code together, and stop.
2. **Pick up to eight concepts**, in this order: due `shaky` ones (oldest "review from" date first), other `shaky` ones, `new` ones (oldest first), then `known` ones not tested for the longest time. When the user names a topic ("review React"), pick only from that topic.
3. **Say what is coming** in two sentences: how many questions, and which topics.
4. **Ask** in rounds of four questions per AskUserQuestion call. Each question tests one concept: what it is for, when to use it, or what goes wrong without it. Build the options from the glossary card and from the mistakes its "Watch out" line names.
5. **After each round**, give the results as the quiz rules say. For each wrong answer, show the full card again in fresh words with a new comparison.
6. **Update `notes.md`** after each round: `known` with `right <date>`, or `shaky` with `wrong <date>` and `review from <date + 3 days>`.
7. **Close** with the score, the concepts to come back to, and when the next review would be useful (the earliest "review from" date).
