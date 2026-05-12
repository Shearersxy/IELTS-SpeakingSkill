# IELTS Speaking Workflows

## Daily Question-Bank Practice

For `/ielts-speaking today`, select questions from the current-season question bank without generating answers by default.

Selection order:

1. Include due review questions first.
2. Add high-priority `new` questions.
3. Ensure Part 2 receives regular coverage because it needs story preparation.
4. Add older saved or practiced questions that have not been reviewed recently.

Default daily set:

- 5 Part 1 questions.
- 1 Part 2 question.
- 3 Part 3 questions.
- 2-4 review questions if due.

Output format:

```text
Today's IELTS Speaking Practice

New Questions
1. Part 1 | Music | Do you like listening to music?
2. Part 2 | Person | Describe a person who inspired you.

Review
1. Part 1 | Reading | Do you enjoy reading books?

Choose a question and run:
/ielts-speaking answer [question_id]
```

## Answer Generation

For `/ielts-speaking answer [question]`, generate a draft only. Never save automatically.

Part-specific generation:

- Part 1: Use profile facts and produce 2-4 sentences.
- Part 2: Inspect `speaking_story_bank.json` first. Use a reusable story only when it genuinely matches the cue card and the user's actual situation; otherwise ask the user targeted questions before drafting.
- Part 3: Select opinion modules from `speaking_opinion_bank.json`; produce a spoken answer with claim, reason, example, and light balance.

Output format:

```text
Draft Answer
[answer]

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

Do not write to any data file during answer generation.

## Adding One Question

For `/ielts-speaking add [part/category/question/cue points]`, add exactly one question to `speaking_question_bank.json`, then help create an approved answer.

Question intake:

1. Parse the part, category, question text, season, priority, and any Part 2 cue points from the user message.
2. If part, category, season, or required Part 2 cue points are missing, ask only for those missing details.
3. Deduplicate by normalized question text. If the question already exists, use the existing record instead of creating a duplicate.
4. Generate a stable question id using part, normalized category, and the next sequence number.
5. Set `status` to `new`, `answer_id` to `null`, `last_practiced` to `null`, and initialize mapping arrays.
6. Use `priority: "high"` only if the user labels it as must-do, high-frequency, current exam, forecast, or important; otherwise use `medium`.
7. Add tags such as `user-added`, `current-season`, and the part.

After the question is added, branch based on the user's preference:

- If the user wants to answer first, show the question and ask them to answer naturally. When they provide an answer, give IELTS 7.5+ feedback and a polished spoken version. Keep iterating with `/ielts-speaking edit [feedback]` until the user is satisfied. Do not record this as formal practice and do not save it until the user runs `/ielts-speaking save` or explicitly says to save.
- If the user wants Codex to draft the answer, inspect the relevant data first. For Part 1, check profile facts and phrase bank. For Part 2, check the story bank before drafting. For Part 3, check opinion modules. If no material is genuinely suitable, ask 3-5 targeted questions about the user's real experience before drafting.
- If the user already includes their answer in the add request, add the question first, then immediately run the self-answer polishing branch.

Story suitability for Part 2:

- A story is suitable only if it matches the main cue-card subject and can naturally cover most cue points without changing important personal facts.
- Do not reuse a story just because the topic category is similar.
- If the best story needs major factual changes, ask the user for a real story instead.

## Editing an Unsaved Draft

For `/ielts-speaking edit [feedback]`, revise the latest draft in the conversation.

If the latest `/ielts-speaking feedback` response showed the saved answer from the answer library, the user may also edit that shown answer. Treat it as the current editable draft, then save it only when the user runs `/ielts-speaking save` or explicitly asks to save the revised version.

Common edit goals:

- Make it more natural.
- Make it shorter.
- Make it longer.
- Make it easier to say.
- Add personal detail.
- Reduce memorized phrasing.
- Improve vocabulary while keeping it spoken.
- Fix grammar.

After editing, show the revised draft and the same two next commands: edit again or save.

Do not write to any data file during draft editing.

## Saving an Approved Answer

For `/ielts-speaking save`, save only the latest draft the user has approved.

Save steps:

1. Find or create the matching question in `speaking_question_bank.json`.
2. Create an `answer_id` if this is a new saved answer.
3. Write the answer to `speaking_answers.json`.
4. If the question already has an answer, append a new version instead of duplicating the answer record.
5. Set the question status to `saved`.
6. Link `answer_id` from the question record.
7. If the question is Part 2, extract the approved story and save or update it in `speaking_story_bank.json`.
8. Add or update the review schedule with `next_review = today + 1 day`.

Do not mark a question as `practiced` unless the user has actually spoken or pasted an attempted answer.

## Part 2 Story Extraction

When saving an approved Part 2 answer, maintain the story bank for future cue-card reuse.

Extraction rules:

1. Identify the core real-world story: person, place, event, object, timeline, conflict or turning point, and final feeling/result.
2. If the answer was built from an existing story, update that story's `usable_for`, `weak_points`, `updated_at`, and source links instead of creating a duplicate.
3. If it is a new story, create a concise story record with title, story type, summary, timeline, emotions, `usable_for`, and source links.
4. Add the current cue-card wording and close paraphrases to `usable_for`, so the story can be reused for later Part 2 questions.
5. Link the story id from the question's `mapped_story_ids` and the answer's `story_ids_used` where possible.

Only extract stories from an answer the user has approved for saving. Do not save a story from an early draft or an answer the user is still revising.

## Reviewing Saved Questions

For `/ielts-speaking review`, select due items from `speaking_review.json`.

Review flow:

1. Show the question only.
2. Ask the user to answer aloud or paste the answer.
3. Do not show the saved answer first unless the user explicitly asks.
4. After the user responds, evaluate with `/ielts-speaking feedback`.
5. After feedback, show the saved answer from the answer library as a reference.
6. Offer the next commands: `/ielts-speaking edit [feedback]` to revise that answer, or `/ielts-speaking save` after the user approves a revised version.

## Feedback and Status Updates

For `/ielts-speaking feedback [user answer]`, score the user's response against IELTS Speaking band descriptors, not against the saved answer. Use the question and the user's speaking goals to judge whether the answer is relevant, natural, and exam-appropriate.

Evaluate:

- Fluency and coherence: smoothness, hesitation, directness, structure, and completion.
- Lexical resource: natural range, precision, collocation, and phrase use.
- Grammatical range and accuracy: sentence control, variety, and error impact.
- Pronunciation: assess only when audio or reliable speech information is available; if the user provides text only, state that pronunciation cannot be fully assessed.
- Personal fit: whether it sounds like the user.

Use IELTS-style band scores when useful. Do not lower or raise the score simply because the answer differs from the saved answer.

After the feedback, show:

- The saved answer from `speaking_answers.json` as `Answer Library Reference`.
- Key differences that are useful for improvement, phrased as optional upgrades rather than scoring criteria.
- Next commands: `/ielts-speaking edit [feedback]` to revise the answer library version, or `/ielts-speaking save` after the user approves a revised version.

Status update rules:

- Keep `saved` if the answer is incomplete, off-topic, or mostly unreadable as spoken output.
- Use `practiced` if the answer is understandable but unstable.
- Use `fluent` if the answer is smooth with only minor issues.
- Use `exam_ready` if the answer is natural, complete, flexible, and easy to recall.

Review schedule:

- Weak attempt: next review tomorrow.
- `practiced`: next review in 2-3 days.
- `fluent`: next review in 5-7 days.
- `exam_ready`: next review in 14 days.

Always record the attempt in `speaking_practice_records.json` when the user provides an answer.

## Importing Current-Season Questions

When importing questions:

1. Preserve the user's season label.
2. Parse Part 1, Part 2, and Part 3 sections.
3. Deduplicate by normalized question text.
4. Generate stable IDs with part, category, and sequence number.
5. Set initial status to `new`.
6. Assign priority when obvious; otherwise use `medium`.

If the season is missing, ask the user for the season before writing.

## Listing and Stats

For `/ielts-speaking list`, support filters by:

- Part.
- Category.
- Status.
- Priority.
- Season.

For `/ielts-speaking stats`, report:

- Total question count.
- Counts by part.
- Counts by status.
- Saved answer count.
- Due review count.
- Part 2 story coverage.
- Weak categories from practice records.
