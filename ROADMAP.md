# Roadmap

## TMATS-vs-recording cross-check

Owner addition: 2026-11-27. Planned; not yet implemented.

Cross-check the TMATS declarations against the recorded data:

- Require every declared channel to be present in the data; report missing channels.
- Report channels present in the data but undeclared in TMATS.
- Report data-type mismatches between TMATS declarations and the recording.
- Report time-source mismatches between TMATS declarations and the recording.

Consume `irig106-core` (`PacketFacts` plus sideband) and `irig106-tmats`.
The validator must not introduce its own recording reader or TMATS parser:
there is one recording reader, in core, and TMATS parsing belongs to
`irig106-tmats`.

Support an external `--tmats` full-replacement override. When supplied, the
external TMATS replaces the recording's TMATS for the cross-check; do not merge
the two. Clearly label the external override in the validation report.

Deliver through quick, small releases: first channel presence and undeclared
channel reporting, then data-type and time-source mismatch reporting. Each
release must preserve the shared-reader/parser boundaries and support the
labelled full-replacement override for the checks it provides.
