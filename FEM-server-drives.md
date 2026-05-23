# FEM Server Drives

The FEM server has multiple drives that differ in capacity, speed, backup options, permissions, and intended use. Here you can find which one to use and when.

---

## Overview of all drives

| Mount point    | Capacity | Backup    | Permissions | Speed       |
|----------------|----------|-----------|-------------|-------------|
| /home          | Small    | No        |             | Fast (NVMe) |
| /scratch       | 3.4TB    | No        |             | Fast (NVMe) |
| /data-raid     | 22TB     | RAID only |             | Slow (HDD)  |
| /data-microlab | 22TB     | No        | microlab    | Slow (HDD)  |
| /data-hdd      | 22TB     | No        |             | Slow (HDD)  |
| /data-nas      |          | Yes       |             | NAS         |


## Storing your data

You should **always** avoid storing your data in /home or /scratch. You should use the following options for data storage:

- `/data-nas` is meant only for limited volumes of essential data you cannot afford to lose. **Never use for computation!** See below.
- `/data-raid` is protected against hardware failures. You should use it for long-term keeping of bigger volumes of important data. It is up to you to back your data up (for instance using ha-bay.ics.muni.cz).
- `/data-microlab` and `/data-hdd` are meant for keeping non-essential reproducible data, such as datasets you can repeatedly download or large by-products of your calculations. 

### Personal data

- `/home` is small and no data should be stored here.
- Move data from `/home` to one of these locations:
  - `/data-hdd` ... not backed up, suitable for things that don't need to be backed up (they are in git, ...) or computational artifacts (e.g. intermediate results) that can be reproduced.
  - `/data-raid` ... simply backed up, suitable for things that need to be backed up in some way.
- A better backup is either `ha-bay` or `/data-nas` — neither should be read from/written to frequently, i.e. don't run computations over them; the shared university disk `ha-bay` is very slow and is therefore suitable for long-term backups that you don't plan to overwrite or load for computations.

#### How to store data:

- Create your personal directory on these disks, e.g. `mkdir /data-hdd/qasar`
- Set permissions on it, e.g. `chmod -R go-rwx /data-hdd/qasar`
- Copy your data here, e.g. `scp -r folder_name /data-hdd/qasar`, or
- Move your data here, e.g. `mv folder_name /data-hdd/qasar`
- You can link any directory into your home, so that it appears to be there but doesn't take up space, e.g. `ln -s /data-hdd/qasar/folder_name ~`

### Shared data

- Shared data typically belongs in `/data-raid`, where it is automatically protected against hardware failure.
- Do not create your own copies of data that we share — link the shared data to the appropriate location in your directory, for example like this: `ln -s /data-raid/fem/SHARE/data/share_release_9.0.0/original/data/ ./SHARE`
- For data you currently have copied from `/data-nas` or linked from `/data-nas`, please link it to `/data-raid` ASAP.

### Git

- **Never commit or push any data or symbolic links to data into git.**

## Workflow for using `/scratch`

Use the `/scratch` workflow only for I/O intensive tasks -- such as working with grid data, using `arrow`, etc.

1. Copy inputs to a temporary directory in `/scratch` before running jobs — fast NVMe speeds up computation.
2. Run your analysis. Keep intermediate files on `/scratch`.
3. When done, move final outputs to a `/data*` drive.
4. Clean up `/scratch` — it is shared and has no backup.


## /data-nas cleaning

If you occupy some space on `/data-nas`, move your data away.

- Use `/data-raid` for data that must be backed up somewhat. Put a copy of your data to [ha-bay.ics.muni.cz](https://it.muni.cz/en/services/network-file-storage/navody/connecting-mu-network-storage-to-windows), too.
- Use `/data-hdd` or `/data-microlab` for your own computational artifacts that can be re-generated.

Do not compute on data stored at `/data-nas` as NAS servers do not handle it well.

---
