Can a Ethernet module can we loadable kernel module
		Yes
Can support for initial RAM file system image be a loadable module
		No, it needs to be loaded at the start of the kernel itself
 What are the steps being able to use a loadable kernel module after editing one of it's source file
		 First remove the module and then reload the module by insmod
	
 lsmod can be used to see all dynamically loaded kernel modules

To view all parameters in the bluetooth modules we need to do

cat /sys/module/bluetooth/parameters/*
 
