# Adding One Question

Use for `/ielts-speaking add [part/category/question/cue points]`.

Read `references/data-contracts.md` for question status and draft-vs-saved-answer rules.

Add exactly one question to `speaking_question_bank.json`, then help create an approved answer.

## Question Intake

1. Parse the part, category, question text, season, priority, and any Part 2 cue points from the user message.
2. If part, category, season, or required Part 2 cue points are missing, ask only for those missing details.
3. Deduplicate by normalized question text. If the question already exists, use the existing record instead of creating a duplicate.
4. Generate a stable question id using part, normalized category, and the next sequence number.
5. Set `status` to `new`, `answer_id` to `null`, `last_practiced` to `null`, and initialize mapping arrays.
6. Use `priority: "high"` only if the user labels it as must-do, high-frequency, current exam, forecast, or important; otherwise use `medium`.
7. Add tags such as `user-added`, `current-season`, and the part.

## After Adding

Show the question and give the user two choices:

1. Answer by themselves first. Ask the user to answer naturally. When they provide an answer, give IELTS feedback at the band set in `speaking_profile.json` -> `profile.speaking_preferences.target_band`, then provide a polished spoken version. Keep iterating with `/ielts-speaking edit [feedback]` until the user is satisfied. Do not record this as formal practice and do not save it until the user runs `/ielts-speaking save` or explicitly says to save.
2. Run `/ielts-speaking answer [question_id]` to prepare an answer draft through the normal Answer Generation workflow.

If the user already includes their answer in the add request, add the question first, then immediately run the self-answer polishing branch.
