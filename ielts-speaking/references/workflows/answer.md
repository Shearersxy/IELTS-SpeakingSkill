# Answer Generation

Use for `/ielts-speaking answer [question_id or question text]`.

Read `references/data-contracts.md` for target band and draft-vs-saved-answer rules. Generate a draft only. Never save automatically.

## Command Behavior

Before drafting, read `speaking_profile.json` -> `profile.speaking_preferences.target_band`; answer language level must match that IELTS Speaking target band.

Material selection before drafting:

1. Locate the selected question in `speaking_question_bank.json`.
2. Read relevant reusable materials before writing the answer:
   - Part 1: inspect `speaking_profile.json`, reusable facts, and useful phrase-bank items.
   - Part 2: inspect `speaking_story_bank.json`, profile facts, and useful phrase-bank items.
   - Part 3: inspect `speaking_opinion_bank.json`, profile facts, and useful phrase-bank items.
3. If suitable reusable materials exist, extract the best fact/story/opinion options and show them to the user. Ask the user to choose which material to use, or to add their own material. Wait for confirmation before generating the answer.
4. If no suitable reusable material exists, ask the user to provide personal material or ideas first.
5. If the user provides material, generate the answer from that material.
6. Only if the user has no material to provide, generate with conservative, clearly editable content.

## Part-Specific Generation

### Part 1

Use profile facts and produce 2-4 sentences.

### Part 2

- Produce a story-based Part 2 answer of about 1.5-2 minutes.
- Before drafting, identify:
  - cue 1-3 as setup cues
  - the final "and explain why/how..." cue as the main development cue
- Keep cue 1-3 brief and factual. Do not expand them beyond setup unless the final cue is genuinely hard to develop naturally.
- Put most development into the final cue. Use 2 clear ideas, and develop each idea with a pyramid structure: point -> reason/explanation -> specific detail, example, feeling, contrast, or result.
- After every Part 2 draft, include:
  - Cue-point coverage
  - Core topic vocabulary
  - Practical spoken phrases

### Part 3

Select opinion modules from `speaking_opinion_bank.json`; produce a spoken answer with claim, reason, example, and light balance.

## Quality Standards

Prefer answers that are easy for the user to actually say. Avoid essay-style linking, dense clauses, memorized templates, and unnatural advanced words.

For every generated, polished, revised, or reference answer, provide a short keyword and phrase section after the answer. Include useful keywords, word groups, chunks, and natural spoken phrases that help the user recall and reproduce the answer. Keep meanings simple and learner-friendly.

## Output Format

```text
Draft Answer
[answer]

Part 2 additions, when applicable:
- Cue-point coverage
- Core topic vocabulary
- Practical spoken phrases

Key Phrases
- phrase: meaning

Used Materials
- Facts: [...]
- Stories: [...]
- Opinions: [...]

Next
- /ielts-speaking edit [feedback]
- /ielts-speaking save
```
