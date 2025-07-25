# Why Partitioning Is Important for Security?

Partitioning allows you to apply specific security controls to different parts of the filesystem. For example, directories like `/tmp` or `/var` can be mounted with options like `noexec` (to block script execution) or `nosuid` (to prevent privilege escalation).

It also reduces the impact of a compromise — isolating data makes it harder for an attacker to affect the whole system.

Additionally, when encrypting the full system, `/boot` must stay unencrypted, so it needs to be on a separate partition.

### Other Benefits

* Prevents one directory (like `/var` or `/tmp`) from filling up the root filesystem and crashing the system
* Makes it easier to back up and protect user data by isolating /home
* Allows using optimized filesystems like `tmpfs` for `/tmp`, which stores temporary data in RAM and clears it on reboot
