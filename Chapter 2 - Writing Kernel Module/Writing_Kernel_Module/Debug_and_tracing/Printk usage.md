The printk command format is

`printk(<Log Level>, "logmessage");`

The printk logs can be viewed using the command 

`dmesg`

`dmesg -c` will clear all the last debug logs

Usage of printk

![[Pasted image 20240926221417.png]]
These message when compiled and ran as kernel module, we can see output in dmesg
![[Pasted image 20240926221352.png]]