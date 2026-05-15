# Saving An Approved Answer

Use for `/ielts-speaking save`.

Read `references/data-contracts.md` for question status, saved-answer, review interval, and Part 2 story-bank rules.

Save only the latest draft the user has approved.

## Save Steps

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
