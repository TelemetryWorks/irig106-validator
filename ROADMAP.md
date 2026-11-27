# Roadmap

## TMATS-vs-recording cross-check

Owner addition: 2026-11-27. Planned; not yet implemented.

Cross-check the TMATS declarations against the recorded data:

- Report declared enabled channels with no packets separately from declared
  disabled channels, using CHE (INT-054). Short or event-driven recordings may
  legitimately contain no data for an enabled channel; absence alone is not a
  validation failure.
- Report channels present in the data but undeclared in TMATS, excluding channel
  0 and mapping the time channel through its TIMEIN declaration.
- Report data-type mismatches between TMATS declarations and the recording.
  The CDT-name to Chapter 11 data-type mapping must be a reviewed
  `irig106-tmats` interpretation (ADR-0033), never invented in the validator.
- Report time-source mismatches using `irig106-time` as the time-packet CSDW
  decoder, per the coordinator's architecture-layering decision. Interpretation
  is edition-dependent: in IRIG 106-04, time CSDW bits 3-0 are EXT, not the time
  source. The validator never parses CSDWs itself.

Consume `irig106-core`, `irig106-types`, `irig106-tmats` and `irig106-time`.
Published `irig106-types` 0.2 and `irig106-core` 0.2 are prerequisites.
`PacketFacts` comes from `irig106-types` and is header-only, built from core's
reader; core provides the `PacketInfo` sideband.
The validator must not introduce its own recording reader or TMATS parser:
there is one recording reader, in core, and TMATS parsing belongs to
`irig106-tmats`.

Which TMATS governs multiple setup records, cut recordings or ambiguous cases
depends on `irig106-tmats`' open governing-record decision (INT-031), including
incomplete/ambiguous access. Never silently use the first setup record.

Support an external `--tmats` full-replacement override. When supplied, the
external TMATS governs the whole file and replaces the recording's TMATS for
the cross-check; do not merge the two. Still parse the in-file setup, including
mid-recording changes, and report its differences from the external TMATS.
Clearly label the external override in the validation report; exact wording
follows the owner's report-format decision. This behavior must remain consistent
with `irig106-tmats` #60.

Deliver through quick, small releases: first channel presence and undeclared
channel reporting, then data-type and time-source mismatch reporting. Each
release must preserve the shared-reader/parser boundaries and support the
labelled full-replacement override for the checks it provides.
