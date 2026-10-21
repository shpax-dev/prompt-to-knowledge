# Files, Descriptors, and Links

A pathname identifies a route through directories. A directory entry maps a name to an inode number. An inode describes a file's metadata and data-block pointers; its names live in directory entries.

## Open files

A process's integer file descriptor refers to an open-file description that tracks state such as current offset and a reference to the file. Two independent `open()` calls can refer to one inode with independent offsets. Descriptors duplicated with `dup()` or inherited through `fork()` can share an open-file description and offset.

Do not store the current descriptor offset conceptually inside the inode. Path lookup is generally done at open time instead of starting from the root for every read on an existing descriptor.

## Hard and symbolic links

A hard link adds another directory entry for the same inode. Updating the file through either name changes the same data. Removing one name decrements the link count; the file can remain accessible through another name or an open descriptor. Reclamation normally waits for both no links and no relevant open references. Ordinary hard links cannot cross file systems and are generally not permitted for directories.

A symbolic link is a separate object containing a target path. Relative targets are resolved relative to the symlink's containing directory. Renaming or removing the target can leave the symlink dangling. Symlinks can cross file-system boundaries and refer to directories.

**Original practice:** create `a`, hard-link `b` to it, then rename `a` to `c`. Names `b` and `c` still refer to the same inode. Replacing the hard link with a symlink whose stored target is `a` would leave that symlink dangling after the rename.

## Visibility versus durability

`write()` may update buffers; `close()` is not a universal power-loss durability guarantee. `fsync()` requests persistence of relevant file data and metadata. A durable rename-based replacement protocol may also need synchronization of the parent directory, depending on the file system and required guarantee. Atomic namespace visibility and crash durability are different properties.

Source: `CS537 - Final Review.pdf`, PDF pp. 16-20; `FA15 - exam.pdf`, pp. 5-6; `FA19 - exam.pdf`, p. 2; `FA24 - exam.pdf`, pp. 6-7. Related: [[review/persistence/file-system-layout-and-traces|on-disk traces]].
