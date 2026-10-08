# Parked work

Things deliberately deferred, with what unblocks them. Add an entry whenever something is parked, and delete it (or move it to "Done") when it's picked up. Dates are UTC.

## Queued

- **Asymmetric Security rogue-agent investigation** (added 2026-09-30 by the owner): <https://www.asymmetricsecurity.com/newsroom/rogue-agent-investigation>. Not yet surveyed. **To incorporate:**
  - Read the report and find its linked data. Add the report and any bulk artifacts to `sources/sources.toml`, acquire and sha-pin them, and survey them in `agents.md` like the other sources.
  - Register it as an incident in `reference.py`, or map it onto an existing one if it covers the same activity.
  - Write an extractor to canonical events, then re-run build → indicators → link. Check it against the existing bridges (relay services, naming shapes, target domains, /16s, OpenAI egress).
  - Add a notebook and update `docs/findings/cross-incident.md`.

- **ARB Research incident catalog** (added 2026-10-08 by the owner): <https://cheats.arbresearch.com/incidents/>. Not yet surveyed. First check whether it is a catalog of other publishers' reports, which may duplicate sources we already have, or a dataset in its own right. Then follow the same incorporation steps as above.
- **swarmcha.se "Chinese agent fleet" post** (added 2026-10-08 by the owner): <https://swarmcha.se/posts/chinese-agent-fleet>. Not yet surveyed. It appears to describe a separate fleet, so it is likely a new incident rather than part of the OpenAI-attributed swarm. Check whether it publishes data, then follow the same incorporation steps as above. Bridges to the existing incidents would be a finding in either direction.

## Waiting on something external

- **Community agent-pastes export** (`termina-swarm-map/agent-pastes-2026-09-08.tar.gz`, linked from rubyhack.ai). `swarm.termina.digital` has returned 503 "public exports are temporarily unavailable" since 2026-09-25; the last check was 2026-09-28. The periodic retry was stopped at the owner's request. Re-run `uv run agent-swarm acquire --only termina-swarm-map` by hand at the start of a work session. If it's still an error, discard the timestamp-only change with `git checkout -- sources/manifest.json`. **When it lands:** survey it in `agents.md`, add an extractor, and fold it into Stage 9 linkage (it's a venue/handle map, so it's likely rich in bridging identifiers).
- **Wayback `searchbot.json` snapshots** (parked 2026-09-26). The CDX index returned 503 on 4 attempts. Rerun `uv run agent-swarm enrich wayback`; it resumes. Also diagnose why 13 of the 51 fetched snapshots don't parse as JSON (likely archive error pages).
- **Pre-July 2026 RubyGems dumps** are in S3 Glacier and not publicly retrievable. Only the maintainers could supply them; see decision 4 in `docs/enrichment-plan.md`.

## Awaiting owner review

- **Stage 10 integration plan** (`docs/enrichment-plan.md`). Nothing is built past the cursory checks until it's reviewed. Its five open decisions: full urlquery crawl, control sample, pusher-id handling, asking RubyGems for Glacier dumps, confidence for account-expanded gems.

## Deferred by decision

- **Stage 8: technique tagging** (parked 2026-09-26). Writing even a report-level technique-category catalog was stopped twice by the assistant's safety classifier, so it's parked rather than attempted in another form. Stage 9 links the incidents on identifiers and timing only, with no technique overlap. **To resume:** decide on a form for the technique layer (e.g. a human-authored catalog committed by the owner, or an existing external taxonomy mapping), then add tagging over structural signals only.

- **Detection rules for attack techniques over payload text** (parked 2026-09-26 by the owner). Stage 8 tags only the technique *categories* the published reports already describe, using structural signals (event types, publisher labels, extracted indicators) with no text-level rule detail. Follow-up:
  - Survey existing detection libraries and rule sets that could run over the payload text (e.g. YARA/Sigma-style rule collections, secret scanners, static analysers for JS/Python/Ruby), and weigh them against writing our own.
  - Define how to run them safely over adversarial text: static only, never executed, outputs aggregated.
  - Decide what precision audit they need before their tags feed findings.
