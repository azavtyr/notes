# Hard and Symbolic Links

* Hard Link points directly to the same inode as the original file
* Symbolic (soft) link is a special file that points to the pathname of another file

### Hard link limitations:

* Cannot link to a file on a different filesystem (must be on the same disk/partition)
* Cannot link to a directory

### Behavior differences:

If the original file was deleted, a hard link will still access the data because it shares the same inode. In contrast, a symbolic link will break and return an error, since it points to a path that no longer exists.
