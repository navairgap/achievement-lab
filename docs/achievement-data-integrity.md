# achievement data integrity

Keep official GitHub achievements separate from user-created profile graphics and project badges. Never infer achievement status from README images. When recording an achievement, retain its source and observation date, distinguish verified facts from user-entered notes, and handle missing fields without inventing values. Avoid storing access tokens or unnecessary personal data in fixtures.

## Suggested observation record

Store observations as structured records rather than inferring status from a screenshot or badge URL:

```json
{
  "kind": "github_achievement",
  "name": "Quickdraw",
  "status": "observed",
  "source_url": null,
  "observed_at": "2026-10-27T00:00:00Z",
  "evidence_type": "user_verified",
  "notes": null
}
```

This is a schema illustration, not a claim that any particular account has earned the named achievement. Use a specific source URL when evidence is available; otherwise mark the source as missing and do not upgrade the record to verified. Keep the observation date separate from the earning date, which may be unknown.

## Observation status transitions

Use a small, explicit status vocabulary such as `unverified`, `observed`, and `verified`. An observation becomes verified only after a source that directly supports the achievement claim has been reviewed; a screenshot of a decorative README badge is not sufficient evidence on its own. Keep verification metadata (reviewed timestamp and evidence reference) separate from the date the achievement was earned, and retain the previous status in an audit history rather than overwriting it silently.

## Import validation

Validate imported records at the boundary. Require a non-empty achievement name and a recognized status, parse observation timestamps as explicit UTC instants, and reject malformed URLs rather than storing them as evidence. Preserve unknown fields in an import report for review, but do not silently map unknown status values to `verified`. Deduplicate using a documented stable key and retain conflicting source records for manual resolution rather than picking one silently.

## Evidence changes and revocation

Evidence can become unavailable or be corrected after an observation is recorded. Keep the original observation and add a new review event when a source link changes, disappears, or no longer supports the claim. Do not silently retain a verified status after the evidence has been revoked; downgrade it to an explicitly reviewed status and preserve the reason and review timestamp. A missing source is not proof that an achievement was never earned, but it is also not sufficient evidence for verification.
