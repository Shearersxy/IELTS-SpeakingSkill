---
name: ielts-speaking
description: IELTS speaking question-bank and material system. Use when the user asks for IELTS Speaking Part 1, Part 2, or Part 3 current-season question management, adding one question to the bank, personalized answer generation, editable answer drafts, approved answer saving, daily practice, answer revision, speaking feedback, review scheduling, progress stats, or commands such as /ielts-speaking, /ielts-speaking today, /ielts-speaking add, /ielts-speaking answer, /ielts-speaking edit, /ielts-speaking save, /ielts-speaking review, /ielts-speaking feedback, /ielts-speaking list, or /ielts-speaking stats.
---

# IELTS Speaking

## Purpose

Manage a personalized IELTS speaking system built around the current-season question bank, approved answer library, reusable personal materials, daily practice, and spaced review.

Do not treat IELTS speaking like IELTS writing. Speaking work should optimize for natural spoken output, personal fit, flexible reuse, and exam-room recall.

## Core Principle

Generate answers as editable conversation drafts first. Do not save generated or edited answers to the formal answer library unless the user explicitly runs `/ielts-speaking save` or clearly says to save the current answer.

When adding a single new question, add the question to the question bank first, then help the user either polish their own answer or build a draft from suitable personal materials. Do not force a story-bank item into an answer unless it genuinely fits the question and the user's actual situation.

For Part 2, after the user approves and saves an answer, extract the reusable story behind it and save or update it in `speaking_story_bank.json` so it can support later cue-card reuse.

Use these five question statuses only:

- `new` - no approved answer has been saved.
- `saved` - an approved answer exists but it is not fluent yet.
- `practiced` - the user has spoken it at least once, but it is unstable.
- `fluent` - the user can say it smoothly but should still review it.
- `exam_ready` - the answer is ready for exam use and only needs light checks.

## Data Files

Default to the current workspace `speakingData/` directory. Create missing files when needed and keep existing user data intact.

- `speakingData/speaking_profile.json` - user background, preferences, constraints, and reusable personal facts.
- `speakingData/speaking_question_bank.json` - current-season IELTS speaking questions.
- `speakingData/speaking_answers.json` - approved saved answers only.
- `speakingData/speaking_story_bank.json` - reusable personal stories, mainly for Part 2.
- `speakingData/speaking_phrase_bank.json` - natural spoken expressions.
- `speakingData/speaking_opinion_bank.json` - Part 3 idea modules.
- `speakingData/speaking_practice_records.json` - practice attempts and feedback.
- `speakingData/speaking_review.json` - review schedule for saved questions.

Read `references/schemas.md` before creating or modifying data files. Read `references/workflows.md` before running command workflows.

## Command Workflows

### `/ielts-speaking` or `/ielts-speaking today`

Create today's practice list from the current-season question bank.

Prioritize in this order:

1. Due review questions from `speaking_review.json`.
2. High-priority `new` questions.
3. New Part 2 questions.
4. Old saved/practiced questions not reviewed recently.

Default daily load:

- 5 Part 1 questions.
- 1 Part 2 question.
- 3 Part 3 questions.
- 2-4 due review questions if available.

Return a concise checklist grouped into `New Questions` and `Review`. Do not generate answers during `today` unless the user asks for a specific answer.

### `/ielts-speaking answer [question_id or question text]`

Generate an answer draft for the selected question and show it in the conversation. Do not save it.

After generating, present:

- Answer draft.
- Key words, word groups, and spoken phrases for every answer, with simple English meanings.
- For Part 2 answers, include core topic vocabulary and reusable phrases after the answer.
- Materials used, such as fact/story/opinion IDs.
- Next commands: `/ielts-speaking edit [feedback]` or `/ielts-speaking save`.

Do not update question status after generation.

### `/ielts-speaking add [part/category/question/cue points]`

Add one question to `speaking_question_bank.json`. If part, category, season, or Part 2 cue points are missing, ask only for the missing details needed to create a clean record.

After adding the question, ask whether the user wants to answer first or wants Codex to draft an answer.

- If the user answers first: give IELTS 7.5+ speaking feedback and a polished spoken version. Iterate with `/ielts-speaking edit [feedback]` until the user is satisfied, then save only after `/ielts-speaking save` or an explicit save request.
- If Codex drafts the answer: inspect `speaking_profile.json`, `speaking_story_bank.json`, and relevant phrase/opinion data first. Use an existing story only when it is clearly suitable; if no story or fact fits, ask targeted questions about the user's real experience before drafting.
- If the added question is Part 2: when the final answer is saved, extract or update a reusable story in `speaking_story_bank.json` and link it from the question/answer where possible.

### `/ielts-speaking edit [feedback]`

Revise the latest unsaved answer draft in the conversation according to the user's feedback. Do not save it.

If the latest `/ielts-speaking feedback` response showed the saved answer from the answer library, treat that answer as the editable draft when the user asks to revise it. Do not overwrite the saved answer until the user runs `/ielts-speaking save` or explicitly asks to save the revised version.

If there is no available draft or recently shown saved answer in the current conversation, ask the user to provide the answer or run `/ielts-speaking answer [question]` first.

### `/ielts-speaking save`

Save the latest approved draft from the conversation into `speaking_answers.json`. Then update the question bank and review schedule.

If saving an edited version of an already saved answer, append a new version to that answer rather than creating a duplicate.

### `/ielts-speaking review`

Review due saved questions. Show the question first without showing the saved answer. Ask the user to answer aloud or paste the answer, then use `/ielts-speaking feedback`.

After feedback, show the saved answer from the answer library as a reference. Review materials must include the saved/reference answer plus its keywords, word groups, chunks, and spoken phrases with simple meanings. The user may edit that answer and then run `/ielts-speaking save` to append the revised version to the answer library.

### `/ielts-speaking feedback [user answer]`

Score the user's answer against IELTS Speaking band descriptors, not against the saved answer. Give concise feedback on fluency and coherence, lexical resource, grammatical range and accuracy, pronunciation when available, and personal fit.

Use the saved answer only after feedback as a reference answer from the answer library. Show it after the feedback so the user can decide whether to edit and re-save it.

Record the attempt in `speaking_practice_records.json`. Update status and next review date based on performance.

### `/ielts-speaking list`

List questions or answers. Support filters for part, category, status, priority, and season.

### `/ielts-speaking stats`

Summarize current-season coverage, status counts, saved answer count, due review count, Part 2 story coverage, and weak categories.

## Answer Quality Standards

Prefer answers that are easy for the user to actually say. Avoid essay-style linking, dense clauses, memorized templates, and unnatural advanced words.

Use personal details from `speaking_profile.json` and saved facts when available. If personal data is missing, use conservative placeholders and make them easy to edit.

For Part 1, produce 2-4 natural sentences. For Part 2, produce a 1.5-2 minute story-based answer. For Part 3, produce a spoken analytical answer with claim, reason, example, and light balance.

For every generated, polished, revised, or reference answer, provide a short keyword and phrase section after the answer. Include useful keywords, word groups, chunks, and natural spoken phrases that help the user recall and reproduce the answer. Keep meanings simple and learner-friendly.

### Part 2 Answer Requirements

Part 2 answers must follow this structure:

- Clear introduction: start by telling the examiner exactly what the answer will talk about.
- Cue-point first: before drafting, identify every cue question in the card. The answer must explicitly cover every cue point, usually in the same order as the card.
- Simple and direct cue answers: answer the smaller cue questions briefly and naturally. Do not expand every small cue point equally.
- Pyramid structure: use the pyramid structure inside each individual idea, not by mixing several ideas together. Each main idea should be developed separately in this pattern: point -> reason/explanation -> specific detail, example, feeling, contrast, or result.
- Final cue priority: the final "And explain why/how..." cue question should carry the deepest development. Give one or two clear main ideas, but develop each idea independently with its own pyramid structure instead of blending the reasons together.

After every Part 2 answer draft, provide a brief cue-point match showing how the answer covers each bullet point. Then provide core topic vocabulary and phrases in addition to the required keyword and phrase section for every answer. Include practical spoken expressions that fit the topic, with simple meanings and, when useful, short example uses from the answer.
