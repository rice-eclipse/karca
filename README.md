# kARCA: An engine controller PCB for Rice Eclipse

kARCA is Rice Eclipse's next generation of engine controller boards.
Its intended use is for data collection and valve actuation control on the Titan and Proxima rocket
motors.

It was designed in 2022 by Rice Eclipse.

## Usage

This board was designed in [KiCAD](https://www.kicad.org/).
Exports (such as gerber files) should be stored in the `target` directory (covered by gitignore).

It connects to a Raspberry Pi running our custom controller software,
[`slonk`](https://github.com/rice-eclipse/slonk), which in turn interfaces with the user dashboard
[`slonkboard`](https://github.com/rice-eclipse/slonkboard).

## Further documentation

A full overview of the design of kARCA is available in [docs/karca.md](docs/karca.md).
