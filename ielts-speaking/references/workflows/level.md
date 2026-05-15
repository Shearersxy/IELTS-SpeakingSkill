# Target Level

Use for `/ielts-speaking level` and `/ielts-speaking level [target band]`.

Read `references/data-contracts.md` for the `target_band` path and default value.

For `/ielts-speaking level`, show the current answer generation target from `speaking_profile.json` -> `profile.speaking_preferences.target_band`. If it is missing, initialize it as `7.0-7.5`.

For `/ielts-speaking level [target band]`, update only `profile.speaking_preferences.target_band` in `speaking_profile.json`, preserving all other profile fields and facts.

Accepted target formats include:

- A single band, such as `6.5`, `7`, or `8`.
- A band range, such as `7.0-7.5`.
- A short natural phrase that clearly contains a band target, such as `set to 7.5`.

When the user sets a level:

1. Normalize obvious values to a compact string, for example `7` -> `7.0`, and `7-7.5` -> `7.0-7.5`.
2. Keep the default as `7.0-7.5` unless the user explicitly changes it.
3. Confirm the updated level in the response.
4. Do not change saved answers, question statuses, review schedules, or practice records.
