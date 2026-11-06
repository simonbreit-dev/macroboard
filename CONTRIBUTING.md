# Contributing

When reporting an issue or proposing a change, include the affected revision,
KiCad version, and a reproducible description. For hardware issues, distinguish
findings in the design files from measurements on an assembled board.

Use KiCad 10 and open the project through `electronics/makroboard.kicad_pro`.
Keep the schematic and PCB in sync, and include any required local symbols and
footprints. Document changes to fabrication requirements and GPIO assignments.

Before submitting a change, run ERC and DRC, including schematic/PCB parity
checks. Commands are provided in [electronics/README.md](electronics/README.md).
Include the results and any remaining warnings in the proposal. Keep generated
reports, Gerbers, assembly exports, and personal editor or agent files local.

For new third-party material, provide its source and license, and update
[THIRD_PARTY.md](THIRD_PARTY.md). Original hardware contributions are submitted
under CERN-OHL-P-2.0; changes to third-party library material must preserve its
applicable terms.
