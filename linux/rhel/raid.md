RAID stands for Redundant Arrays of Independent Disks
can achieve greater performance or redundancy than the collection of drives can offer when operated individually. RAID is implemented as a layer in between the raw drives or partitions and the filesystem layer.

Redundancy is meant to help increase the availability of your data. 
Striping is the process of dividing the writes to the array over multiple underlying disks. When data is striped across an array, it is split into chunks, and each chunk is written to at least one of the underlying devices. 

Chunk Size: When striping data, chunk size defines the amount of data that each chunk will contain. 

Parity: Parity is a data integrity mechanism implemented by calculating information from the data blocks written to the array

common RAID levels are:

RAID 0 combines two or more devices by striping data across them.
advantage of this is that since the data is distributed, the whole power of each device can be utilized for both reads and writes.

Disadvantage Since data is split up and divided between each of the disks in the array, the failure of a single device will bring down the entire array and all data will be lost. 

RAID 1 is a configuration which mirrors data between two or more devices. each device has a complete set of the available data, data will still be accessible as long as a single device in the array is still functioning properly. 

RAID 5, data is striped across disks in much the same way as a RAID 0 array.read performance benefits from the ability to read from multiple disks at once RAID 5 parity, a level of redundancy can be achieved at the cost of only a single disk's worth of space. So, four 100G drives in a RAID 5 array would yield 300G of usable space (the other 100G would be taken up by the distributed parity information).



RAID 6 similar to RAID 5, but with double parity information. array can withstand any two disks failing. 

RAID 10
a minimum of four disks is required to form a RAID 1+0 array
RAID 1+0 arrays have the high performance characteristics of a RAID 0 array, but instead of relying on single disks for each component of the stripe, a mirrored array is used, providing redundancy. This type of configuration can handle disk failures in any of its mirrored RAID 1 sets so long as at least one of disk in each RAID 1 remains available
