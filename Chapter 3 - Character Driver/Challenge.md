Execute a chardriver program which has arguments, a filename and the major version associated with the file.
1. So first build the chardriver using kernel's and user's makefile.
2. After building the ko file load the module using insmod by passing the arguments
3. Then create the character file in /tmp/ using mknod <major_ver> <minor_ver>. Make sure you do umask 0 to enable default write permission.
4. write something to the file and do cat to see the content of the file.
5. rmmod the module
6. Check the logs using dmesg.

commands used:
![[Pasted image 20241003212821.png]]
dmesg output:
![[Pasted image 20241003212846.png]]