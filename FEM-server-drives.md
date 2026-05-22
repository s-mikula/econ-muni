# FEM Server Drives

The FEM server has multiple drives that differ in capacity, speed, backup options, permissions, and intended use. Here you can find which one to use and when.

---

## Overview of all drives

| Mount point    | Capacity | Backup    | Permissions | Speed       |
|----------------|----------|-----------|-------------|-------------|
| /home          | Small    | No        |             | Fast (NVMe) |
| /scratch       | 3.4TB    | No        |             | Fast (NVMe) |
| /data-nas      |          | Yes       |             | NAS         |
| /data-microlab | 22TB     | No        | microlab    | Slow (HDD)  |
| /data-hdd      | 22TB     | No        |             | Slow (HDD)  |
| /data-raid     | 22TB     | RAID only |             | Slow (HDD)  |


## Storing your data

You should **always** avoid storing your data in /home or /scratch. You should use the following options for data storage:

- `/data-nas` is meant only for limited volumes of essential data you cannot afford to lose. In the medium run, we should free as much space on `/data-nas` as possible.
- `/data-raid` is protected against hardware failures. You should use it for long-term keeping of bigger volumes of important data. It is up to you to back your data up (for instance using ha-bay.ics.muni.cz).
- `/data-microlab` and `/data-hdd` are meant for keeping non-essential reproducible data, such as datasets you can repeatedly download or large by-products of your calculations. 


## Workflow for using /scratch

1. Copy inputs to `/scratch` before running jobs — fast NVMe speeds up computation.
2. Run your analysis. Keep intermediate files on `/scratch`.
3. When done, move final outputs to a `/data*` drive.
4. Clean up `/scratch` — it is shared and has no backup.

---
