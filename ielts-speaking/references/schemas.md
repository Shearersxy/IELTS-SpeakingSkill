# IELTS Speaking Data Schemas

Use JSON files under the workspace `speakingData/` directory. Preserve existing fields when updating records. Add new fields only when they support speaking practice, answer reuse, or review scheduling.

## `speaking_question_bank.json`

Stores the current-season IELTS Speaking question bank.

```json
{
  "season": "2026 Jan-Apr",
  "questions": [
    {
      "id": "p1_music_001",
      "part": "part1",
      "category": "Music",
      "question": "Do you like listening to music?",
      "status": "new",
      "priority": "high",
      "tags": ["hobby", "daily life"],
      "answer_id": null,
      "mapped_story_ids": [],
      "mapped_opinion_ids": [],
      "last_practiced": null,
      "created_at": "2026-04-27",
      "updated_at": "2026-04-27"
    }
  ]
}
```

Allowed `part` values:

- `part1`
- `part2`
- `part3`

Allowed `status` values:

- `new`
- `saved`
- `practiced`
- `fluent`
- `exam_ready`

Allowed `priority` values:

- `high`
- `medium`
- `low`

When adding a single user-provided question:

- Reuse the existing top-level `season` unless the user provides a different season.
- Deduplicate by normalized question text before creating a new record.
- Add `tags` including `user-added`, `current-season`, and the part.
- Optional fields are allowed when useful: `source_section`, `source_topic_zh`, `cue_points`, and `p2_type`.
- For Part 2, store cue-card bullet points in `cue_points` when available and link reusable stories in `mapped_story_ids` after the answer is approved.

## `speaking_answers.json`

Stores approved saved answers only. Do not write generated drafts here until the user explicitly saves.

```json
{
  "answers": [
    {
      "id": "ans_20260427_001",
      "question_id": "p1_music_001",
      "part": "part1",
      "question": "Do you like listening to music?",
      "current_version": 1,
      "versions": [
        {
          "version": 1,
          "answer": "Yes, I do. I usually listen to music on my way to school, especially when I feel a bit tired. It helps me relax and puts me in a better mood.",
          "change_note": "Initial approved version",
          "created_at": "2026-04-27"
        }
      ],
      "key_phrases": [
        {
          "en": "puts me in a better mood",
          "meaning": "makes me feel happier or more relaxed",
          "example": "It helps me relax and puts me in a better mood."
        }
      ],
      "personal_facts_used": ["fact_music_commute"],
      "story_ids_used": [],
      "opinion_ids_used": [],
      "status": "saved",
      "created_at": "2026-04-27",
      "updated_at": "2026-04-27"
    }
  ]
}
```

For Part 2 answers, update `story_ids_used` after extracting or matching a reusable story from the approved answer.

## `speaking_profile.json`

Stores the user's stable background and personal details for natural answer generation.

```json
{
  "profile": {
    "study_or_work": "",
    "major_or_job": "",
    "city": "",
    "hobbies": [],
    "personality": [],
    "speaking_preferences": {
      "target_band": "7.0-7.5",
      "answer_style": "natural and easy to say",
      "avoid": ["overly formal wording", "memorized-sounding answers"]
    }
  },
  "facts": [
    {
      "id": "fact_music_commute",
      "category": "Music",
      "fact": "I usually listen to music on my way to school.",
      "usable_for": ["music", "relaxation", "daily routine"]
    }
  ]
}
```

## `speaking_story_bank.json`

Stores reusable personal stories, mainly for Part 2.

```json
{
  "stories": [
    {
      "id": "story_first_presentation",
      "title": "My first English presentation",
      "story_type": "experience",
      "summary": "I gave my first English presentation at school and became more confident after preparing for several days.",
      "timeline": [
        "I had to give a presentation in English.",
        "I felt nervous because I had little experience.",
        "I practiced several times and asked a friend for feedback.",
        "The final presentation went better than expected."
      ],
      "emotions": ["nervous", "relieved", "proud"],
      "usable_for": [
        "a time you felt proud",
        "a difficult thing you did",
        "a skill you learned"
      ],
      "source_question_ids": ["p2_proud_001"],
      "source_answer_ids": ["ans_20260427_002"],
      "weak_points": [],
      "created_at": "2026-04-27",
      "updated_at": "2026-04-27"
    }
  ]
}
```

For Part 2 story extraction:

- Create or update stories only from user-approved saved answers.
- Prefer updating an existing story when the core person/place/event/object is the same.
- Use `usable_for` to store the current cue card plus reusable paraphrases.
- Use `source_question_ids` and `source_answer_ids` when available to keep traceability.

## `speaking_phrase_bank.json`

Stores natural spoken phrases. Prefer phrases the user can actually say smoothly.

```json
{
  "phrases": [
    {
      "id": "phrase_confidence_boost",
      "theme": "confidence",
      "en": "It gave me a real confidence boost.",
      "meaning": "It made me feel much more confident.",
      "example": "Giving that presentation gave me a real confidence boost.",
      "difficulty": "medium"
    }
  ]
}
```

## `speaking_opinion_bank.json`

Stores Part 3 idea modules, not essay paragraphs.

```json
{
  "opinions": [
    {
      "id": "op_tech_learning_flexible",
      "theme": "technology",
      "claim": "Technology makes learning more flexible.",
      "reason": "People can study whenever they have spare time.",
      "example": "Many students use apps to review vocabulary on the subway.",
      "balance": "However, it still depends on whether they can stay focused.",
      "usable_for": ["education", "technology", "self-study"]
    }
  ]
}
```

## `speaking_practice_records.json`

Stores practice attempts and feedback.

```json
{
  "records": [
    {
      "id": "practice_20260427_001",
      "date": "2026-04-27",
      "question_id": "p1_music_001",
      "answer_id": "ans_20260427_001",
      "part": "part1",
      "user_response": "Yes, I like music because it makes me relaxed...",
      "scores": {
        "fluency_coherence": 6.5,
        "lexical_resource": 6.5,
        "grammar_range_accuracy": 7,
        "pronunciation": null
      },
      "pronunciation_note": "Text-only response; pronunciation was not fully assessed.",
      "problems": ["The answer is understandable but slightly repetitive."],
      "next_action": "Add one specific personal detail.",
      "status_after": "practiced"
    }
  ]
}
```

Score practice attempts against IELTS Speaking criteria rather than against the saved answer. If the user provides text only, keep `pronunciation` as `null` or add a clear note explaining that pronunciation could not be fully assessed.

## `speaking_review.json`

Stores review schedules for saved questions.

```json
{
  "schedule": [
    {
      "question_id": "p1_music_001",
      "answer_id": "ans_20260427_001",
      "part": "part1",
      "question": "Do you like listening to music?",
      "status": "saved",
      "next_review": "2026-04-28",
      "last_reviewed": null,
      "review_count": 0
    }
  ]
}
```

Review intervals:

- `saved`: review in 1 day.
- `practiced`: review in 2-3 days.
- `fluent`: review in 5-7 days.
- `exam_ready`: review in 14 days.
