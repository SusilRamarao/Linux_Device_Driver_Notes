Well it has three main functions
1. driver_read()
2. driver_write()
3. driver_init() - > where we register the device driver as character driver

In the driver_init() -> we create file_operation instance where we attach the read and write function of file operation instance to our functions.

==Functions used while reading a block of data==
`copy_to_user ->  Copy a block of data into user space.`

`Copy data from kernel space to user space.`

`Returns number of bytes that could not be copied. On success, this will be zero.`
[copy_to_user (bham.ac.uk)](https://www.cs.bham.ac.uk/~exr/lectures/opsys/13_14/docs/kernelAPI/r4037.html)

==Functions while writing a block of data==

`On x86 and x86-64, it simply does a direct read from the userspace address and write to the kernelspace address, while temporarily disabling SMAP (Supervisor Mode Access Prevention) if it is configured.`
`[How does copy_from_user from the Linux kernel work internally? - Stack Overflow](https://stackoverflow.com/questions/8265657/how-does-copy-from-user-from-the-linux-kernel-work-internally)`

