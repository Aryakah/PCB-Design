# ESC project symbol-library search

Date: 2026-11-22

The official Microchip dsPIC33C CAD/CAE archive was downloaded and its two exact dsPIC33CK256MP506 package files were installed under `Libraries/Symbols/Microchip_dsPIC33C/`. They remain in original BXL format because KiCad 10 does not natively import BXL and the supplied Ultra Librarian Reader is a Windows executable unavailable in this environment. No schematic or existing project file was modified.

## Requested components

| Component | Source checked | Downloaded | Pin mapping verified | Installed |
|---|---|---:|---:|---:|
| dsPIC33CK256MP506 (family) | Official Microchip dsPIC33C CAD/CAE archive: `https://ww1.microchip.com/downloads/aemDocuments/documents/MCU16/ProductDocuments/BoardDesignFiles/dsPIC33C_CAD_CAE_Schematic_Symbols.zip` | Yes, original `.bxl` | Not yet; BXL is binary and not decoded | Original BXL installed; not KiCad-resolvable |
| dsPIC33CDVC256MP506-E/M9 (physical SiP, U9A/U9B/U9C) | Same archive and dsPIC product page | No exact CDVC/SiP file in archive; only CK256MP506 MR/PT files | No | No |
| MCP8021 function inside U9B | Official product page CAD Models section; legacy Microchip `HalfBridge Mosfet Driver.zip` also checked | Legacy archive contains MIC460x parts, not MCP8021; no separate component permitted | Pending SiP documentation verification | No separate installation |
| ATA6563 function inside U9C | Official product page CAD Models section; legacy Microchip `CAN.zip` also checked | Legacy archive contains MCP251x/MCP2551 parts, not ATA6563; no separate component permitted | Pending SiP documentation verification | No separate installation |
| SIJA72ADP-T1-GE3 | Official Vishay product page `https://www.vishay.com/en/product/75342/`; CAD/ECAD section checked | No schematic symbol offered; SPICE/thermal models only | N/A | No |

## Other manufacturer-specific parts found in the ESC reference PDF

- PESD2CANFD27V-UX
- PMEG4010CEH,115
- PMEG6020ELRX
- BAS40-04

These were also not found as verifiable downloadable KiCad symbol files during this pass.

## Search results

- Installed KiCad 10 libraries were searched locally; none contains the requested exact part numbers.
- The current KiCad symbol repository was checked for the relevant DSP, CAN, and FET libraries; exact requested part numbers were absent.
- GitHub/source-code searches produced no downloadable KiCad symbol library containing the requested exact parts.
- Direct manufacturer web pages were checked where accessible; no directly downloadable, verified KiCad symbol file was available from the returned resources.
- Microchip’s official archive was downloaded successfully. It contains exact `DSPIC33CK256MP506-X_MR.bxl` and `DSPIC33CK256MP506-X_PT.bxl` symbols, but they are Ultra Librarian BXL binaries rather than KiCad `.kicad_sym` files.
- SnapEDA, Ultra Librarian, and SamacSys pages were checked, but no accessible downloadable result could be verified without an authenticated or interactive component-download session.

## Project configuration

The project-local directory now contains the original Microchip BXL archive and exact package files. No `sym-lib-table` entry was added for BXL because KiCad cannot resolve that format; adding it would create a broken library entry. The final schematic must represent one physical `U9` with units `U9A`, `U9B`, and `U9C`, sharing one SiP footprint. ATA6563 and MCP8021 must not be added as separate physical components.

## Integrity review — 2026-11-22

- All four current schematic files exist: `Microchip-ESC-Recreated-Sheet1.kicad_sch` through `Sheet4.kicad_sch`. KiCad can parse/export each file.
- All four checked-in netlists reference the matching sheet source file. Independent temporary netlist regeneration matched each existing netlist byte-for-byte after ignoring only the generated timestamp.
- No `U9`, `U9A`, `U9B`, `U9C`, `dsPIC33CDVC256MP506`, `MCP8021`, or `ATA6563` instance occurs in the current schematic or netlist files. Therefore the requested shared-U9 architecture is not yet present, and no separate ATA6563/MCP8021 components were introduced.
- U9 pin assignments cannot be compared or verified because no U9 symbol instance exists in the current files. Zero U9 pins are verified.
- The four sheets are standalone files, not a connected hierarchical KiCad project. Their visible label sets are template-like and repeated, while the generated netlists contain predominantly unconnected component pins. No inter-sheet net can therefore be certified as electrically connected.
- The reference PDF’s U9A/U9B/U9C connections—including DSC-to-gate-driver PWM/FAULT/VREG/VBOOT signals and DSC-to-CAN transceiver CAN_STBY/CAN_TXD/CAN_RXD—remain unverified in the current schematic files.

### Connections still unverified

Against `Drone-ESC.pdf`, DS70005664A, pages 15–17:

- U9A→U9B: `PWMAH`, `PWMAL`, `PWMBH`, `PWMBL`, `PWMCH`, `PWMCL`, `FAULT`, `DE2`, and `OE`.
- U9A/U9B power and control: `VREG`, `VBOOT`, `HVDD`, `HVSS`, `GND`, and the three gate-driver phase outputs `PHASE_A`, `PHASE_B`, `PHASE_C`.
- U9A→U9C: `CAN_STBY`, `CAN_TXD`, `CAN_RXD`, `VDD`→`CAN_VIO`, and the external `dsPIC33CDVC256MP506` pin-43 `CAN_RXD` connection.
- Sheet-to-sheet functional signals: PWM outputs to the inverter, phase/shunt feedback, bus and phase-voltage sensing, temperature/ADC signals, `AD_PWM_IN1`, `AD_PWM_IN2`, `UART_PWM`, debug/programming signals, and CAN connector/protection nets.
- All exact U9 pin numbers and all discrete-device pin endpoints remain unverified because the corresponding symbols are absent.

The exact `dsPIC33CDVC256MP506-E/M9` SiP representation remains unresolved. The official archive still provides only `DSPIC33CK256MP506-X_MR.bxl` and `DSPIC33CK256MP506-X_PT.bxl`; these must not be substituted for the complete SiP.

## Official-source follow-up — 2026-11-22

### Microchip SiP evidence

- `Drone-ESC.pdf` is the downloaded official reference-design user guide,
  SHA-256 `bef7e27501613f289d31ef0cb3e89ef80015fd3e2f305b7301710ab810bbef39`,
  source:
  `https://ww1.microchip.com/downloads/aemDocuments/documents/MCU16/ProductDocuments/UserGuides/Drone-Electronic-Speed-Controller-ESC-Reference-Design-User-Guide-DS70005664.pdf`.
- The guide explicitly identifies U9 as one
  `dsPIC33CDVC256MP506-E/M9` SiP integrating the DSC, MCP8021 gate driver,
  and ATA6563 CAN-FD transceiver (pages 1, 10–12, 23). It is documentation,
  not a CAD library or downloadable symbol.
- The guide's authoritative U9 pin table verifies: DSC VDD 25/41/52, VSS
  26/40/65, AVDD 19, AVSS 20; gate-driver HVDD 7, HVSS 5/66, CAP1 9,
  CAP2 8, VBOOT 4, VREG 6, WAKE 55, VBA/VBB/VBC 58/61/64, PHA/PHB/PHC
  56/59/62, HSA/HSB/HSC 57/60/63, LSA/LSB/LSC 1/2/3; CANH 50, CANL 49,
  CAN_RXD 43, CAN_VDD 48, CAN_VSS 51, and N/C 67. This verifies the
  required U9A/U9B/U9C pin evidence but does not supply a CAD symbol.
- The official Microchip CAD archive URL
  `https://ww1.microchip.com/downloads/aemDocuments/documents/MCU16/ProductDocuments/BoardDesignFiles/dsPIC33C_CAD_CAE_Schematic_Symbols.zip`
  is present locally as
  `Libraries/Symbols/Microchip_dsPIC33C/dsPIC33C_CAD_CAE_Schematic_Symbols.zip`,
  SHA-256 `d9aff169b130a6b0074ad0b44c3994cad527be422fe57d208bded6631b02f7bd`.
  Its 136-entry archive contains no `dsPIC33CDVC256MP506`, no SiP package,
  and no combined U9A/U9B/U9C symbol. It contains CK256MP506 MR/PT BXLs,
  which remain intentionally uninstalled as KiCad libraries and must not be
  used as a substitute. Microchip's product page was inaccessible to this
  environment (HTTP 403); no authenticated CAD download was obtained.

### Exact discrete-part CAD results

| Part | Verified source/evidence | Result/blocker |
|---|---|---|
| SIJA72ADP-T1-GE3 | Vishay product page `https://www.vishay.com/en/product/75342/` links exact Ultra Librarian record `https://vendor.ultralibrarian.com/vishay/embedded?q=library/ecad/&vdrSearch=SIJA72ADP-T1-GE3&docId=75342`. The record exposes exact part ID 5354768, symbol preview `TRANS_N-MOSFET5 1`, and footprint `SOT_DP-T1-GE3_VIS`, with KiCad v5/v6+ and STEP/IGES options. | Verified exact CAD listing, but no file installed: UL requires registration/authorization, terms acceptance, and enabled third-party cookies; export failed in this environment. Vishay datasheet/package documents are available, but are not CAD symbols. |
| PESD2CANFD27V-UX | Official Nexperia product URL `https://www.nexperia.com/product/PESD2CANFD27V-UX` | Page returned HTTP 403; no authenticated/downloadable exact CAD artifact or pin-verified symbol obtained. |
| PMEG4010CEH,115 | Official Nexperia product URL `https://www.nexperia.com/product/PMEG4010CEH` | Page returned HTTP 403; no authenticated/downloadable exact CAD artifact or pin-verified symbol obtained. |
| PMEG6020ELRX | Official Nexperia product URL `https://www.nexperia.com/product/PMEG6020ELRX` | Page returned HTTP 403; no authenticated/downloadable exact CAD artifact or pin-verified symbol obtained. |
| BAS40-04 | Official Nexperia product URL `https://www.nexperia.com/product/BAS40-04` | Page returned HTTP 403; no authenticated/downloadable exact CAD artifact or pin-verified symbol obtained. |

No new artifact was installed in this follow-up; no `.kicad_sch` file was
edited. The exact SiP CAD symbol and the four Nexperia CAD downloads remain
blocked. The only concrete discrete CAD lead is the Vishay-sponsored exact
Ultra Librarian record, whose registration-gated export must be supplied by
the user or obtained in an authenticated session.

## Public-source/archive follow-up — 2026-11-22

### Official Microchip GitHub archives

The public Microchip GitHub organization was searched through the GitHub REST
API, including exact `dsPIC33CDVC256MP506` repository searches. These are
legitimate official firmware/documentation sources, but neither is a CAD
package:

| Repository | Exact URL | Retrieved source evidence | Format/result |
|---|---|---|---|
| Drone ESC firmware | `https://github.com/microchip-pic-avr-solutions/dspic33cdvc256mp506-esc-drone` | `https://api.github.com/repos/microchip-pic-avr-solutions/dspic33cdvc256mp506-esc-drone/git/trees/main?recursive=1`; tree-manifest SHA-256 `9f88b44ae8a97f04a738350103f106c85ccadc8b7a9e952d3a448f30f990d71a` | C/MCC Melody project, YAML, text, images; no `.kicad_sch`, `.kicad_sym`, `.bxl`, CAD archive, schematic, PCB, or 3-D model. GitHub release `1.0.0` has no assets: `https://api.github.com/repos/microchip-pic-avr-solutions/dspic33cdvc256mp506-esc-drone/releases/347858163`. |
| dsPIC33CDVC256MP506 MC development board AN1292 | `https://github.com/microchip-pic-avr-solutions/sip-mc-development-board-an1292-dspic33cdvc256mp506` | `https://api.github.com/repos/microchip-pic-avr-solutions/sip-mc-development-board-an1292-dspic33cdvc256mp506/git/trees/main?recursive=1`; tree-manifest SHA-256 `0603bfcc8f90d08567b234adaeef9351ff8d65021d4cb73ec5ece24c0ab9c5d5` | C/MPLAB project, README, images, AN1292 PDF, XLS; no CAD/schematic/library artifact. |
| dsPIC33CDVC256MP506 MC development board AN1299 | `https://github.com/microchip-pic-avr-solutions/sip-mc-development-board-an1299-dspic33cdvc256mp506` | `https://api.github.com/repos/microchip-pic-avr-solutions/sip-mc-development-board-an1299-dspic33cdvc256mp506/git/trees/main?recursive=1`; tree-manifest SHA-256 `326556ecca16071baec3ddc5e33a30d34c45814875091f51da71b9ed6fdac82b` | C/MPLAB project, README, images, AN1299 PDF, XLS; no CAD/schematic/library artifact. |

The official ESC README independently states that the board uses one
`dsPIC33CDVC256MP506` SiP integrating the DSC, MCP8021, and ATA6563:
`https://raw.githubusercontent.com/microchip-pic-avr-solutions/dspic33cdvc256mp506-esc-drone/main/README.md`.
This confirms identity/architecture, not a CAD symbol. No source above is
installed under `Libraries/Symbols/`.

### BXL conversion investigation

Ultra Librarian's official FAQ confirms that the Free Reader is desktop
software and instructs users to uninstall/reinstall the Windows installation
(`C:\UltraLibrarian`) for updates; it also documents export through the
desktop reader:
`https://www.ultralibrarian.com/faq`.
The page does not provide a Linux-native converter or a public standalone BXL
decoder. Direct reader URL variants tested (`/ulreader`, `/reader`, and
`/Download/UL_Reader`) returned HTTP 404. Consequently the two official
Microchip BXL files remain original binary artifacts:

| File | SHA-256 | Conversion status |
|---|---|---|
| `Libraries/Symbols/Microchip_dsPIC33C/DSPIC33CK256MP506-X_MR.bxl` | `4df8bff64cc90c8452a204dbd52c626bc9848581fab48269cff468e8f58b44a2` | No conversion; CK-only and not the CDVC SiP. |
| `Libraries/Symbols/Microchip_dsPIC33C/DSPIC33CK256MP506-X_PT.bxl` | `95db30b400e8f470445e18be08920c5872c6e595300faba43f7809a12b902023` | No conversion; CK-only and not the CDVC SiP. |

No unofficial BXL parser or fabricated conversion utility was installed.
KiCad's official library download page
(`https://www.kicad.org/libraries/download/`) supplies KiCad library releases,
not a BXL reader. A Windows Ultra Librarian Reader export, if obtained by the
user, would still require complete pin-by-pin comparison against DS70005664A
before any use, and cannot satisfy the missing combined U9A/U9B/U9C SiP
requirement unless the exact SiP source is present.

### Resulting blocker

The public official archives add useful identity and firmware evidence but
contain no original ESC CAD package and no exact
`dsPIC33CDVC256MP506-E/M9` multi-unit symbol. No exact symbol name, CAD pin
mapping, package variant, or installable KiCad format was found. The shared
U9A/U9B/U9C requirement therefore remains externally blocked; CK BXL,
dsPIC33CK256MP506, MCP8021, and ATA6563 must not be substituted or placed as
separate physical components.

## Final review disposition — 2026-11-22

The independent reviewer accepted the source-search evidence and confirmed
that no schematic, symbol, footprint, netlist, or ERC artifact is approved
for integration. The project remains an externally blocked partial work
product; the BXL files remain source evidence only and are not registered or
used as KiCad libraries.
