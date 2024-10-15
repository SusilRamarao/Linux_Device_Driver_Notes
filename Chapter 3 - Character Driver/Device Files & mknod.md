There are two types of device files
1. Character device files which is used to read/write character
2. Block device files where we can read a certain block, such as mounting a drive. ( Need clarity )

**mknod** is used to create a device file
`mknod name c | b major minor`
c - character
b - block

`umask 0` allows to create files with write permission by default.



