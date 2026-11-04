# Hardware

I used two Lenovo mini PCs and an HP desktop I already had. The [photo](../photos/README.md) shows how they're arranged.

| Server | Machine | CPU | RAM | Storage |
| --- | --- | --- | --- | --- |
| Bedrock | Lenovo ThinkCentre M710q | i5-7500T, 4 cores / 4 threads | 16 GB DDR4 | 120 GB NVMe |
| ATM10 | Lenovo ThinkCentre M720q | i5-8500T, 6 cores / 6 threads | 16 GB DDR4 | 128 GB WD PC SN520 NVMe |
| Paper | HP ProDesk 600 G2 SFF | i5-6500 | 16 GB DDR4 | 240 GB SSD |

## Moving things around

I originally had Bedrock on the M720q and considered using the M710q for ATM10. After comparing them, I swapped their roles. The M720q's six-core i5 made more sense for the heavier modded server, while the M710q could handle Bedrock.

I moved Bedrock first, checked that it worked on the M710q, then set up ATM10 on the M720q. The HP became the third server when I moved Paper off Windows.

The hostnames after the moves were `mc-bedrock`, `mc-atm10`, and `mc-paper`.

## Storage and usable memory

The HP has a 240 GB SSD, which Linux reports as about 223.6 GiB. It also reports around 14 GiB usable RAM out of the installed 16 GB. Its root logical volume initially used only 100 GiB.

The M710q's NVMe showed about 119.2 GiB, but root initially had only about 58 GiB. I expanded the LVM allocation during setup.

Some of my earlier notes call the M720q drive 120 GB; the more specific hardware recap lists a 128 GB WD PC SN520. I've kept that model here rather than treating the rounded figures as exact measurements.
