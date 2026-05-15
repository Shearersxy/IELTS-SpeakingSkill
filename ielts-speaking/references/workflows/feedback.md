# Feedback And Status Updates

Use for `/ielts-speaking feedback [user answer]`.

Read `references/data-contracts.md` for target band, status, and review schedule rules.

Score the user's response against IELTS Speaking band descriptors, not against the saved answer. Use the question and the user's speaking goals to judge whether the answer is relevant, natural, and exam-appropriate.

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
