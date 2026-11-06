# Makroboard

A macropad with twelve keys, two rotary encoders with push buttons, twelve
addressable RGB LEDs, and an RP2040. The 80 × 82 mm PCB uses four copper layers
and has a USB-C connector on its top edge.

This repository contains the editable electronics design. Firmware, an app, and
an enclosure are not included yet. The design **has not been tested as a physical
prototype**; USB communication, power supplies, and controls must be verified on
an assembled board.

## Getting started

1. Install KiCad 10, including its standard libraries. The design was edited with
   KiCad 10.0.7.
2. Open [`electronics/makroboard.kicad_pro`](electronics/makroboard.kicad_pro).
3. Read the [electronics documentation](electronics/README.md) and, before
   manufacturing, the [fabrication requirements](electronics/FABRICATION-NOTES.txt).

Project-specific symbols and footprints are included. The optional 3D models for
the USB-C connector and encoders are not included in the published project; all
pads, holes, and electrical connections are included.

## Files and exports

- `electronics/`: KiCad project, schematics, PCB, and library tables.
- `electronics/lib/footprints/`: local footprints used by the PCB.
- `LICENSE`, `LICENSES/`, `THIRD_PARTY.md`: license texts, attribution, and
  licensing notes.

Gerbers, drill files, assembly exports, previews, check results, and local agent
files are not versioned. Existing local files remain excluded by `.gitignore`.
Manufacturing files are exported from KiCad for the relevant revision; commands
are provided in the electronics documentation.

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md). Keep hardware changes and documentation
consistent, and preserve the license notices.

## License and attribution

Copyright © 2026 Simon Breit. Original hardware contributions and the associated
project documentation are licensed under **CERN-OHL-P-2.0**; the full text is in
[LICENSE](LICENSE).

Third-party material retains its respective terms. The Raspberry Pi reference
design is licensed under MIT, KiCad library material under CC-BY-SA-4.0 with the
KiCad exception, and marbastlib key footprints under CERN-OHL-P-2.0.
The encoder footprints match those from
[ubiqueIoT/rotary-encoder-PEC11R](https://github.com/ubiqueIoT/rotary-encoder-PEC11R).
See [THIRD_PARTY.md](THIRD_PARTY.md) for further attribution and licensing notes.
CERN-OHL-P-2.0 does not grant additional rights to this third-party material.
