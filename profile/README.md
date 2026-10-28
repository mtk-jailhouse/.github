# MediaTek Jailhouse

The [Jailhouse](https://github.com/siemens/jailhouse) partitioning hypervisor
for MediaTek Genio platforms. Jailhouse lets Linux and a real-time operating
system such as Zephyr run side by side on one SoC, each with its own CPU cores,
memory and devices. On the Genio 510 and Genio 700 EVKs, Zephyr runs next to
Linux in a Jailhouse cell.

## Get started

<a href="https://github.com/mtk-zephyr/genio-docs/releases/latest/download/genio-jailhouse-zephyr-guide.pdf"><img src="https://raw.githubusercontent.com/mtk-zephyr/genio-docs/v1.0/assets/guide-preview.png" alt="Pages of the user guide: the cover, the system overview, and running a Zephyr image" width="100%"></a>

**[User guide (PDF)](https://github.com/mtk-zephyr/genio-docs/releases/latest/download/genio-jailhouse-zephyr-guide.pdf):
Running Zephyr alongside Linux with Jailhouse on Genio 510 and Genio 700 EVKs.**
Build the IoT Yocto image with Jailhouse, flash the board, build Zephyr and the
Genio samples, and run Zephyr in a Jailhouse cell, step by step. This is user
guide 1.0, for the release `mtk-v1.0.0`. The pre-release `v26.1-rc1` has its
own guide,
[user guide 26.1-rc1](https://github.com/mtk-zephyr/genio-docs/releases/tag/v26.1-rc1),
whose PDF was revised on 2026-10-28 for the Zephyr tags.

## Repositories

| Repository | Content |
|---|---|
| [jailhouse](https://github.com/mtk-jailhouse/jailhouse) | Jailhouse with the Genio support. Development and releases are on the default branch `scarthgap`, named after the IoT Yocto release line it targets; a release is named after the IoT Yocto release it ships with. Pre-release: tag `v26.1-rc1`. Previous release: tag `mtk-v1.0.0` on the branch `mtk-v1.0` |

## What the Genio support adds

- **Cells for the Genio 510 and Genio 700 EVKs:** a root cell for Linux, and
  Zephyr cells on a Cortex-A55, on a Cortex-A78, on two cores (SMP), with the
  audio front end, and with memory shared with Linux for RPMsg.
- **Shared register blocks, mediated per pin:** the MT8188 GPIO, external
  interrupt (EINT) and clock gate registers are shared by Linux and the cells.
  Each cell can only read and change the fields of the pins and clock gates it
  owns. The pin configuration registers stay with Linux.
- **Shared interrupts:** an EINT is delivered to the cell that owns its pin.
- **A watchdog for each Zephyr cell:** each cell gets a watchdog of its own,
  with the registers of the MT8188 watchdog, while Linux keeps the hardware
  watchdog. A cell that misses its deadline, or requests a reset, fails
  alone; the SoC and the other cells keep running.
- **Restarting failed cells:** the daemon `jailhouse-cell-restartd` runs on
  Linux and restarts the cells that fail, as its configuration file names
  them.
- **A tick from Linux:** the Jailhouse driver gives the hypervisor a tick
  through the `tick` hypercall, every 100 ms by default. The hypervisor
  checks the cells' watchdog deadlines on it.
- **Reset cause:** after a restart, a cell reads from its watchdog why its
  previous run ended: a watchdog timeout or a reset request. Zephyr reports
  it through its `hwinfo` API.
- **GICv3 and SMC handling for cells:** redistributor gaps of disabled cores,
  the pending state of a cell's SGIs, and per-cell restrictions on calls to
  the secure world.

## Supported boards

| Board | SoC | CPUs |
|---|---|---|
| Genio 510 EVK | MT8370 | 4× Cortex-A55, 2× Cortex-A78 |
| Genio 700 EVK | MT8390 | 6× Cortex-A55, 2× Cortex-A78 |

## Related

- [mtk-zephyr](https://github.com/mtk-zephyr): Zephyr for MediaTek Genio, and
  the [documentation](https://github.com/mtk-zephyr/genio-docs)
- [Jailhouse](https://github.com/siemens/jailhouse): the upstream project
- [IoT Yocto](https://genio.mediatek.com/doc/iot-yocto/latest/sw/yocto/get-started.html): the Linux BSP for Genio
