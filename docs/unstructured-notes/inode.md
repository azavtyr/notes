# Inode

An inode is a data structure that stores all metadata about a file except its name. The number of inodes limits how many files and directories a filesystem can hold.

It contains:

* File size
* Device ID
* User ID and Group ID
* Permissions
* Timestamps
* Data location (not the filepath)
