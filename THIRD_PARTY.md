# Third-party material and licenses

The CERN-OHL-P-2.0 license in `LICENSE` covers Simon Breit's original hardware
contributions and the associated project documentation. It does not replace
licenses or required permissions for third-party material. The following notices
apply to that material.

## Raspberry Pi RP2040 Minimal Design Example

- Copyright holder: Raspberry Pi Ltd, Copyright © 2026. The original schematic
  credits Mike Stimson as its author.
- Source: [RP2040 Minimal KiCad design](https://datasheets.raspberrypi.com/rp2040/Minimal-KiCAD.zip),
  archive `RPI-RP2040-MINIMAL_R3-S1_public`, downloaded on 2026-11-06.
- License and original disclaimer: [LICENSES/RaspberryPi-MIT.txt](LICENSES/RaspberryPi-MIT.txt),
  copied from the source archive without changes other than line endings.
- Parts of the controller subsheet, its symbols and footprints, and seven
  embedded STEP models are used:
  `generic_capc1005x50.step`, `generic_capc2012x125.step`,
  `generic_resc1005x35.step`, `generic_soic127p790x195-8.step`,
  `generic_xtalcc320x250x80-4.step`, `rpi_rp2040.step`, and
  `samtec_tsw-102-07-g-s.step`.
- The embedded models were verified to be byte-identical to the official source
  archive. The archive permits redistribution of its included third-party
  elements without additional license terms; its original disclaimer is retained.
- Changes for Makroboard: USB-C and ESD protection, LDO and component selection,
  matrix/encoder/LED connections, project-local libraries, and a custom four-layer
  layout. On 2026-11-06, unused templates, models, and metadata were removed, and
  non-portable datasheet paths were cleared. This cleanup did not change the
  electrical connections.

The Raspberry Pi PDF manuals have a different license from these CAD files.
No PDF manuals are included in this repository.

## KiCad library material

- Copyright holders: the respective KiCad library contributors.
- Sources: [KiCad symbols](https://gitlab.com/kicad/libraries/kicad-symbols),
  [KiCad footprints](https://gitlab.com/kicad/libraries/kicad-footprints), and
  [KiCad 3D models](https://gitlab.com/kicad/libraries/kicad-packages3D).
- License: [CC-BY-SA-4.0](LICENSES/CC-BY-SA-4.0.txt) with the exception described in
  [LICENSES/KiCad-Libraries.md](LICENSES/KiCad-Libraries.md). The exception applies
  to use in an electronic design; redistribution of a library collection remains
  subject to the library license terms.
- The local libraries contain copies and adaptations of standard symbols and
  footprints, including passive components, diodes, LEDs, and IC packages. Changes
  include project-specific names, component fields, orientations, and footprint
  adjustments. Original rights are retained.
- Standard models referenced through `${KICAD10_3DMODEL_DIR}` are supplied by the
  KiCad installation and are not separately distributed in this repository.

## marbastlib: key footprints

- Authors: ebastler, Marble, and the marbastlib contributors.
- Source: [ebastler/marbastlib](https://github.com/ebastler/marbastlib),
  [`marbastlib-xp-choc.pretty/SW_choc_v2_1u.kicad_mod`](https://github.com/ebastler/marbastlib/blob/main/footprints/marbastlib-xp-choc.pretty/SW_choc_v2_1u.kicad_mod).
- The original local KiCad history explicitly references
  `PCM_marbastlib-xp-choc:SW_choc_v2_1u`.
- License: [CERN-OHL-P-2.0](LICENSE), as stated in the
  [upstream license](https://github.com/ebastler/marbastlib/blob/main/LICENSE).
- Used in `SW1_SW_choc_v2_1u.kicad_mod` through `SW12_SW_choc_v2_1u.kicad_mod`
  and the corresponding PCB footprints. The copies have project-specific names,
  fields, and orientations. Their geometry was unchanged by the cleanup on
  2026-11-06.

## Encoder footprints

- Source: [ubiqueIoT/rotary-encoder-PEC11R](https://github.com/ubiqueIoT/rotary-encoder-PEC11R).
- Used in `SW13_Bourns-PEC11R-4220F-S0024-0-0-0.kicad_mod` and
  `SW14_Bourns-PEC11R-4220F-S0024-0-0-0.kicad_mod` under
  `electronics/lib/footprints/Makroboard_PCB.pretty/`, and in the PCB.
- The copies were updated to the current KiCad format and given project-specific
  names and fields. Pad and drawing geometry match the source.
- The source repository has no explicit license statement. This project does not
  relicense the original data under CERN-OHL-P-2.0.

The external STEP files for the USB-C connector and encoders, along with the
associated source note, are no longer versioned because license documentation
is missing. Their 3D model references were removed from the PCB and local
footprints. Existing local copies are excluded by `.gitignore`.
