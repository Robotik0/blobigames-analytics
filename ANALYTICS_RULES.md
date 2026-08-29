# Blobigames Analytics Query Rules

Use these rules whenever answering analytics questions from this repository.

## Source of truth
- Repository: `Robotik0/blobigames-analytics`
- Link index: `public/query/index.json`
- Per-link history: `public/query/links/*.json`
- Each per-link file contains `domain`, `id`, `path`, `createdAt`, `days`, and `totalClicks`.

## All-time clicks
- For an all-time query, use the link's `totalClicks` as the authoritative archived lifetime total.
- Never reconstruct an all-time total by summing daily archive data when a current lifetime value is available.
- Never use unrelated aggregate fields such as generic `total`, `score`, `count`, or another link's value.

## Date-range queries
- Resolve each requested path through `public/query/index.json`.
- Fetch the corresponding per-link file(s).
- Sum only the values in `days` whose dates fall inside the requested inclusive range.
- Do not count dates earlier than `createdAt`.
- For multiple links, calculate each link independently and then sum the results.
- Preserve the domain with the link ID; paths are not assumed globally unique.

## Rankings
- All-time rankings use `totalClicks`.
- Date-range rankings use the sum of `days` inside the requested range.
- Never mix all-time and date-range totals.

## Validation
- If a value conflicts with a known/current Short.io lifetime total, flag it instead of reporting the conflicting value as fact.
- If the requested data cannot be retrieved completely, say so rather than guessing.
- The historical daily archive is a date-range source; it is not the authoritative source for lifetime totals.

## Canonical workflow
1. Read this file before performing a non-trivial analytics query.
2. Resolve paths via `public/query/index.json`.
3. Fetch only the required per-link files.
4. Apply the appropriate all-time or date-range rule above.
5. State the exact date range used when the request is not all-time.
