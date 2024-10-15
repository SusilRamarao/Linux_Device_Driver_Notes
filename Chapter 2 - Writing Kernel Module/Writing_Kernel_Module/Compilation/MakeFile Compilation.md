*Compiling One File*
To compile a kernel module C code, we need to have the keyword

`obj-m += skeleton.o`

skeleton.o is a object file from skeleton.C, which is a template file.

*Compiling Multiple File*
To compile multiple kernel module C code, we need to have the keyword

`multifile-objs += first.o second.o`
`obj-m += multifile.o`

In this case we have two more kernel code files first.o and second.o, these needs to be added to keyword

`multifile-objs`

and then we need to compile using 

`obj-m` for the main file which uses other two supporting kernel files.



