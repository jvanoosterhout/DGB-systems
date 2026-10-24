# Documentation instructions

## Purpose and boundaries

DGB-systems describes the shared context, architecture and useful design history of the project family. HMD-DGB and each controller repository remain authoritative for their software, hardware, configuration, wiring, tests and operating instructions. Link to those details; do not copy or move them here.

Keep this documentation lightweight and useful. Record shared or reusable needs, requirements, decisions and attempts—not a catalogue of every controller feature. Do not create records, files or relationships just to complete a template.

## Where information belongs

- `README.md`: project entry point, components and a concise system overview.
- `docs/system-context.md`: needs, motivation and system boundaries.
- `docs/requirements.md`: desired behavior and constraints.
- `docs/design-decisions.md`: meaningful choices, reasons and trade-offs.
- `docs/design-attempts.md`: solutions actually built, configured, tested or operated, including what was learned.

The README already contains a system diagram. Keep the architecture there or in the system context; create a separate architecture document only if it adds useful detail.

## Records

Use IDs in the form `<SCOPE>-<TYPE>-<SEQUENCE>`:

- Scopes: `SYS` (system family), `SW` (generic HMD-DGB software), `HW` (controller hardware and integration).
- Types: `REQ` (requirement), `DD` (design decision), `DA` (design attempt).
- Check the relevant file for the next number. Never reuse or renumber an assigned ID; gaps are fine.
- Add `Applies to` when a record concerns a specific project or component.

Use only these statuses:

- `draft`: candidate or unresolved; not an agreed baseline.
- `accepted`: intentionally adopted, but not necessarily implemented.
- `implemented`: believed present or actually tried. This is not independent verification or a safety claim.
- `deprecated`: no longer applicable or current; retain its history.

Give each record a clear current description and a `### History` list with meaningful, dated changes (newest first). Keep the same ID for clarifications; for a substantially different requirement or choice, create a new ID and link the records. Preserve abandoned attempts and explain their outcomes.

## Working rules

- Inspect relevant current project sources before recording claims. Cite repository sources with links; identify maintainer recollection as such.
- Distinguish needs, intended behavior, decisions, tried solutions and current implementation. Do not infer that a proposal is implemented or tested.
- Resolve conflicting or missing evidence explicitly; mark current details as TBD rather than guessing.
- Keep tests, operational evidence and electrical-safety checks with the implementation. Do not add verification records or claim that `implemented` means verified.
- Use English for new documentation, relative links within this repository and full GitHub links to other repositories.
- Update the README or documentation map when adding or moving documentation. Keep links working and remove links when their target is removed.
