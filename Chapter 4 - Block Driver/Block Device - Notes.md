Type of block drivers:
1. Loop - mounting an image inside of an already mounted filesystem
2. SCSI ( used for most disks )
3. RAM disk
4. RAID
**Mounting:**
Mounting is associating a file system with a directory in the system.

Block Device Requests:
1. Block layer makes requests to the driver block and buffers block
2. Block driver serves the request in it's request_queue
3. The Kernel can do I/O with a file by using only the disk cache when data is present.

Scheduling:
Refer
[Kernel/Reference/IOSchedulers - Ubuntu Wiki](https://wiki.ubuntu.com/Kernel/Reference/IOSchedulers)
mq-deadline, Kyber, BFQ, none


