# Microchip ESC recreation verification report

Date: 2026-11-22

## Scope

Reference source: `Drone-ESC.pdf`, official Microchip DS70005664A.

This report records the current verification boundary. It does not claim the
schematic recreation is complete.

## Accepted evidence

- All four recreated schematic sheets and the root schematic parse and export.
- Existing sheet netlists match independently regenerated netlists after
  timestamp normalization.
- PDF export succeeds.
- The official reference guide confirms one physical
  `dsPIC33CDVC256MP506-E/M9` SiP containing U9A/U9B/U9C functionality.
- Official reference pin evidence is recorded in `SYMBOL_SEARCH_REPORT.md`.
- No separate ATA6563 or MCP8021 components were introduced.
- The official Microchip archive contains only CK-family BXL files and no
  complete CDVC SiP symbol.

## Rejected integration

Independent review rejected all schematic, symbol, footprint, netlist, and ERC
integration. No artifact was approved because:

1. No U9/U9A/U9B/U9C instance exists in the current schematic files.
2. The four sheets are standalone and predominantly unconnected.
3. Strict ERC reports violations: 141, 135, 142, 79, and 141 for Sheets 1–4
   and root respectively.
4. Exact symbols remain unavailable for the SiP and several discrete parts.

## Remaining limitations

- Exact `dsPIC33CDVC256MP506-E/M9` multi-unit KiCad representation,
  footprint, and pin mapping are missing.
- `SIJA72ADP-T1-GE3` export is blocked by authenticated Ultra Librarian access.
- Nexperia CAD pages for `PESD2CANFD27V-UX`, `PMEG4010CEH`,
  `PMEG6020ELRX`, and `BAS40-04` returned HTTP 403.
- U9 internal nets, inter-sheet nets, references, values, footprints, and
  visual fidelity remain unverified.

No schematic or original project file was modified during this verification.

## Final reviewer disposition

The independent final review accepted the source-search evidence and rejected
all schematic, symbol, footprint, netlist, and ERC integration. The project is
closed for this autonomous cycle as an externally blocked partial work
product, not as a completed faithful recreation.
