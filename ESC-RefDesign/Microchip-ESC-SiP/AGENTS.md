# Microchip ESC project instructions

## Scope

This repository recreates the Microchip dsPIC33CDVC256MP506 System-in-Package
ESC reference design from:

`https://ww1.microchip.com/downloads/aemDocuments/documents/MCU16/ProductDocuments/UserGuides/Drone-Electronic-Speed-Controller-ESC-Reference-Design-User-Guide-DS70005664.pdf`

The four reference schematic pages are Sheets 1–4 in that document. Preserve
the original design, topology, component identities, references, values,
packages, pin numbers, and electrical connections.

## Mandatory U9 architecture

U9 is one physical `dsPIC33CDVC256MP506-E/M9` SiP with one shared reference,
one shared package footprint, and these schematic units:

- U9A — dsPIC33CK256MP506 DSC function
- U9B — integrated MCP8021 MOSFET gate-driver function
- U9C — integrated ATA6563 CAN-FD transceiver function

ATA6563 and MCP8021 must never be placed as separate physical ICs. Preserve
all documented internal U9A/U9B/U9C interconnections.

Do not substitute `dsPIC33CK256MP506` for the complete SiP.

## Symbol and library policy

- Use only existing, verified manufacturer or third-party symbols.
- Never invent, redraw, manually create, or silently simplify symbols.
- Verify every symbol's pin numbers against an authoritative datasheet or the
  reference design before use.
- Keep all downloaded or converted libraries project-local under
  `Libraries/Symbols/`.
- Register only verified KiCad libraries through a project-local
  `sym-lib-table` using `${KIPRJMOD}`.
- Do not register unsupported BXL or other formats as KiCad libraries.
- Record source URL, original format, converted format, symbol name, pin
  verification, and installation status in `SYMBOL_SEARCH_REPORT.md`.

## File protection and Git safety

Before any change:

1. Run `git status --short --branch`.
2. Identify original files versus generated/recreated work.
3. Work on a dedicated branch.
4. Create a checkpoint commit or clearly documented checkpoint before a
   significant schematic/library change when the working tree permits it.

Never delete, reset, restore, overwrite, or modify original project files.
Never run destructive Git commands or `sudo`. Never push without explicit
permission. Authenticated manufacturer downloads and destructive operations
are genuine blockers requiring the user.

The recreated files are not permission to alter original source files.

## Autonomous workflow

The `esc-orchestrator` is the coordinator. For every cycle it must:

1. Read Git status, `AGENTS.md`, all three progress/report files, blockers,
   current schematics, netlists, ERC reports, and the reference PDF.
2. Select the highest-priority unfinished achievable task.
3. Delegate manufacturer/source work to `esc-researcher`.
4. Delegate native KiCad drawing/netlist/ERC work to `esc-engineer`.
5. Send every proposed result to `esc-reviewer` before integration.
6. Reject reviewer findings that are unresolved or unverified.
7. Integrate only verified changes, preserving U9 architecture and file safety.
8. Run KiCad parse/ERC/netlist checks after integration.
9. Update all three progress files with evidence and percentage status.
10. Start the next achievable task without asking the user to continue.

If an external blocker prevents progress, record it precisely and continue
with independent achievable work. Do not claim completion while blockers or
unverified connections remain.

## Persistent progress files

Maintain:

- `SYMBOL_SEARCH_REPORT.md`
- `SYMBOL_SEARCH_PROGRESS.md`
- `Microchip-ESC-Recreated-PROGRESS.md`

Use a percentage bar and evidence-based checklist. Never mark a task complete
without a reproducible verification result.

## Review requirements

`esc-reviewer` is read-only for schematic and library content. It must compare
the recreated sheets against the reference PDF, verify pin mappings and
inter-sheet nets, inspect ERC/netlists, and list every unresolved connection.
It must reject assumptions and fabricated symbols.

## Completion criteria

Completion requires all four sheets to be faithful and saved, exact verified
symbols/libraries installed project-locally, U9A/U9B/U9C represented as one
physical U9, all references/values/pins/nets verified, inter-sheet wiring
verified, ERC and netlists clean or explicitly justified, and all progress
files updated. If any criterion is blocked, report the blocker rather than
claiming completion.
