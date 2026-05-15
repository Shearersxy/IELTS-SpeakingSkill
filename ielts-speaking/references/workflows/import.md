# Importing Current-Season Questions

Use when importing current-season IELTS Speaking questions into the question bank.

Read `references/data-contracts.md` for question status rules.

When importing questions:

1. Preserve the user's season label.
2. Parse Part 1, Part 2, and Part 3 sections.
3. Deduplicate by normalized question text.
4. Generate stable IDs with part, category, and sequence number.
5. Set initial status to `new`.
6. Assign priority when obvious; otherwise use `medium`.

If the season is missing, ask the user for the season before writing.
