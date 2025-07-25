# Disk Naming Scheme on Linux

Disk devices are named using the pattern: `/dev/xxyz`

* `xx` – Device type (e.g. `sd` for SCSI/SATA, `nvme` for NVMe, `vd` for virtual disks)
* `y` – Device letter (order of detection: a, b, c, etc.)
* `z` – Partition number (e.g. 1, 2, 3)
