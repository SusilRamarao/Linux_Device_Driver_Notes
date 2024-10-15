We try to use the kernel's `makefile` since it contains the necessary headers to build the kernel module.

`make -C (To access other directory makefile) /lib/modules/<linux-kernel-version>/build M=$PWD (This is to indicate kernel makefile to use out makefile where we build the module) modules`

NOTE: Make sure do `make clean` before doing make to cleanup the previously created objects.

After running make of that module, you should be able to see the `.ko` object, which can be loaded using `insmod` and removing it by `rmmod`

The `insmod` will load the module based on the return value from `init` as 0 is success and rest of the negative values have an generic kernel error message. If negative value is returned then the kernel will not be loaded.


