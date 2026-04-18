# Assignment 6 Results

## Test Output
$ testsymlink
PASS: symlink created
PASS: read through symlink
PASS: loop detected (open failed)
testsymlink done

All 3 of the tests passed.

## What I Did
Added `#define T_SYMLINK 4` to `stat.h` to give symlinks their own inode
type separate from files and directories.

## symlink Syscall
Wired up the syscall the usual way, `usys.S`, `user.h`, `syscall.h`, and
`syscall.c`. The actual implementation is in `sys_symlink` in `sysfile.c`.
It grabs the target and linkpath from userspace with `argstr`, creates a new
inode of type `T_SYMLINK` using `create()`, then writes the target path into
the inode's data blocks with `writei`.

I also modified 'sys_open' to follow symlinks. I added a while loop to check if the inode is a symlink. If it is, it reads the target path out of thedata blocks, releases the current inode, and
calls 'namei' on the target. This repeats until we hit a real file. 