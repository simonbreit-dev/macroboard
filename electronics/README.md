# Electronics

Revision **TOP-USB**, 2026-10-17: 80 × 82 mm, four copper layers, nominal finished
PCB thickness of 1.0 mm. Twelve keys form a 4×3 matrix with 19.05 mm key spacing;
the top row is 23 mm from the lower 3×3 block. Two encoders with push buttons sit
at the lower right, with USB-C on the top edge.

## Opening the project

Open `makroboard.kicad_pro` with KiCad 10. The files were edited with KiCad 10.0.7.
The KiCad standard libraries are required, along with the included
`Makroboard.kicad_sym` symbol library and the footprints under
`lib/footprints/Makroboard_PCB.pretty/`. The library tables use project-relative
paths.

`makroboard.kicad_sch` is the main schematic;
`RPI-RP2040-MINIMAL_R3-S1.kicad_sch` is the required controller subsheet, adapted
for Makroboard. Its filename identifies the original reference design. See
[THIRD_PARTY.md](../THIRD_PARTY.md) for attribution and licenses.

The optional external 3D models for USB-C and the encoders are not included.
Other components use embedded reference models and KiCad standard models.
Enclosure and control dimensions must be checked against manufacturer data;
no enclosure design is included.

## Assembly and fabrication

67 SMD/hybrid components are on Bottom; the two required 100 nF capacitors C20/C21
are on Top. C20/C21 use 0805 packages and can be hand-soldered after single-sided
factory assembly. One suitable part is Yageo CC0805KRX7R9BB104, LCSC C49678,
100 nF, 50 V, X7R, 10%.

R3 remains DNP. Keys, encoders, and the optional BOOT header J2 are assembled
separately. Test pads are not purchased components. The LEDs are reverse-mount
SK6812MINI-E devices on Bottom, shining through the PCB toward Top; confirm the
exact variant, pinout, and permitted brightness against the datasheet.

Include the [fabrication requirements](FABRICATION-NOTES.txt) with every
manufacturing order. In particular, the manufacturer must confirm 90 Ω ±10% USB
differential impedance for its actual four-layer stackup.

## Firmware interface

Firmware and an app are not included. The following assignments describe the
current design; they are not instructions for firmware that is already available.

| Signal | GPIO | RP2040 pin |
|---|---:|---:|
| MATRIX_COL_1 | 0 | 2 |
| MATRIX_COL_2 | 18 | 29 |
| MATRIX_COL_3 | 17 | 28 |
| MATRIX_ROW_1 | 3 | 5 |
| MATRIX_ROW_2 | 22 | 34 |
| MATRIX_ROW_3 | 11 | 14 |
| MATRIX_ROW_4 | 6 | 8 |
| LED_DATA | 8 | 11 |
| ENC_1_A | 29 | 41 |
| ENC_1_B | 28 | 40 |
| ENC_1_PUSH | 9 | 12 |
| ENC_2_A | 26 | 38 |
| ENC_2_B | 19 | 30 |
| ENC_2_PUSH | 24 | 36 |

The LED chain starts below SW3 at the upper right and follows a serpentine path
down the board:

| LED index (zero-based) | LED | Key | Matrix row | Matrix column |
|---:|---|---|---:|---:|
| 0 | D1 | SW3 | 1 | 3 |
| 1 | D2 | SW2 | 1 | 2 |
| 2 | D3 | SW1 | 1 | 1 |
| 3 | D4 | SW4 | 2 | 1 |
| 4 | D5 | SW5 | 2 | 2 |
| 5 | D6 | SW6 | 2 | 3 |
| 6 | D7 | SW9 | 3 | 3 |
| 7 | D8 | SW8 | 3 | 2 |
| 8 | D9 | SW7 | 3 | 1 |
| 9 | D10 | SW10 | 4 | 1 |
| 10 | D11 | SW11 | 4 | 2 |
| 11 | D12 | SW12 | 4 | 3 |

## Checks and exports

Run these commands from the repository root. On macOS, add
`/Applications/KiCad/KiCad.app/Contents/MacOS/` to `PATH` to use `kicad-cli`.
Outputs remain local and are excluded by `.gitignore`.

```sh
mkdir -p electronics/checks electronics/generated/gerbers
kicad-cli sch erc --format json --severity-all --exit-code-violations \
  -o electronics/checks/erc.json electronics/makroboard.kicad_sch
kicad-cli pcb drc --format json --schematic-parity --refill-zones \
  --exit-code-violations -o electronics/checks/drc.json \
  electronics/makroboard.kicad_pcb
kicad-cli pcb export gerbers \
  --layers F.Cu,In1.Cu,In2.Cu,B.Cu,F.Mask,B.Mask,F.Silkscreen,B.Silkscreen,F.Paste,B.Paste,Edge.Cuts \
  --use-drill-file-origin --subtract-soldermask --check-zones \
  -o electronics/generated/gerbers/ electronics/makroboard.kicad_pcb
kicad-cli pcb export drill --format excellon --drill-origin plot \
  --excellon-separate-th --excellon-oval-format route \
  -o electronics/generated/gerbers/ electronics/makroboard.kicad_pcb
```

Always generate Gerbers, drill files, and any BOM/placement files from the same
project revision, and inspect them together in KiCad/Gerber Viewer. For new
assembly exports, explicitly define which parts are factory-assembled and which
are hand-soldered. The assembler must verify machine rotation offsets against
the pin-1 markings. Old local exports are not an approved manufacturing package.

The project intentionally disables some ERC/DRC rules, such as missing
courtyards. Interpret a successful check within that configuration; it does not
verify enclosure fit or hardware functionality.

## First power-up

The design **has not been tested as a physical prototype**. After completing
assembly, including C20/C21, check for solder bridges, monitor current draw, and
verify the +3V3 and +1V1 supplies.

To enter the ROM USB bootloader, connect J2 pin 1 to GND, briefly pull RUN at TP3
to GND and release it, then release BOOT. `RPI-RP2` should appear. Alternatively,
use TP1 SWCLK, TP2 SWDIO, TP3 RUN, TP4 GND, and TP5 as the 3.3 V reference for SWD.
Start LED testing at low brightness. Power-up, flashing, USB communication, and
key/encoder/LED operation must be verified on an assembled board with suitable
firmware.
