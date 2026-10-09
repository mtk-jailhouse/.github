# MediaTek Jailhouse

The [Jailhouse](https://github.com/siemens/jailhouse) partitioning hypervisor
for MediaTek Genio platforms. Jailhouse lets Linux and a real-time operating
system such as Zephyr run side by side on one SoC, each with its own CPU cores,
memory and devices. On the Genio 510 and Genio 700 EVKs, Zephyr runs next to
Linux in a Jailhouse cell.

## Get started

<a href="https://github.com/mtk-zephyr/genio-docs/releases/latest/download/genio-jailhouse-zephyr-guide.pdf"><img src="https://raw.githubusercontent.com/mtk-zephyr/genio-docs/main/assets/guide-preview.png" alt="Pages of the user guide: the cover, the system overview, and running a Zephyr image" width="100%"></a>

**[User guide (PDF)](https://github.com/mtk-zephyr/genio-docs/releases/latest/download/genio-jailhouse-zephyr-guide.pdf):
Running Zephyr alongside Linux with Jailhouse on Genio 510 and Genio 700 EVKs.**
Build the IoT Yocto image with Jailhouse, flash the board, build Zephyr and the
Genio samples, and run Zephyr in a Jailhouse cell, step by step.

## Repositories

| Repository | Content |
|---|---|
| [jailhouse](https://github.com/mtk-jailhouse/jailhouse) | Jailhouse with the Genio support: development on the default branch `mtk-genio-dev`, releases on `mtk-v1.0` (current release: tag `mtk-v1.0.0`) |

## What the Genio support adds

- **Cells for the Genio 510 and Genio 700 EVKs:** a root cell for Linux, and
  Zephyr cells on a Cortex-A55, on a Cortex-A78, on two cores (SMP), with the
  audio front end, and with memory shared with Linux for RPMsg.
- **Shared register blocks, mediated per pin:** the MT8188 GPIO, external
  interrupt (EINT) and clock gate registers are shared by Linux and the cells.
  Each cell can only read and change the fields of the pins and clock gates it
  owns. The pin configuration registers stay with Linux.
- **Shared interrupts:** an EINT is delivered to the cell that owns its pin.
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
