# RAIDmount

Utility script to mount a RAID array at a given mountpoint

## Usage

```
RAIDmount
Usage: raidmount [-h | --help] (-l <n> | --level <n>) (-m <dir> | --mount <dir>) <disk>...

Options:
  -l, --level <n>    RAID level: 0,1,4,5,6,10
  -m, --mount <dir>  Mountpoint for RAID array
  -h, --help         Show this help menu


Each RAID level requires a minimum amount of disks and has a max disk fault tolerance:
    +-------+-------+---------+
    | level | disks | faults  |
    +-------+-------+---------+
    |   0   |   2+  |    0    |
    |   1   |   2+  |  n - 1  |
    |   4   |   3+  |    1    |
    |   5   |   3+  |    1    |
    |   6   |   4+  |    2    |
    |  10   |   4+  |  n / 2  |
    +-------+-------+---------+
(Note: RAID level 10 (1+0) requires an even number of disks
and can tolerate at most one disk failure per mirrored pair.)

Use 'missing' to denote a disk as corrupt or missing, e.g.
  raidmount -l 6 -m raid disk1.img missing disk3.img missing

Mountpoint will be created if it does not exist.
If it exists but is not a directory, the program will exit.

At the end, a cleanup script is generated to run when done with the mounted disk.


Examples:
  raidmount -l 1 -m raid1 missing disk2.img missing missing
  raidmount --level=5 --mount raid5 disk3.img missing disk2.img
  raidmount -l 10 -m raid10 disk1 missing missing disk4 disk5 disk6


Failure examples:
  raidmount -l 0 -m raid0 missing disk2.img
    RAID 0 has no fault tolerance

  raidmount -l 4 -m raid4 disk1 missing missing
    RAID 4 tolerates at most one disk failure

  raidmount -l 6 -m raid6 disk1 disk2 disk3
    RAID 6 requires at least four disks

  raidmount -l 10 -m raid10 disk1 disk2 disk3 disk4 disk5
    RAID 1+0 requires an even number of disks

  raidmount -l 10 -m raid10 disk1 disk2 missing missing
    RAID 1+0 tolerates at most one disk failure per mirrored pair
```

## Contributing

Contributions are more than welcome.
To contribute, please fork the project, develop your feature on a new branch, and create a new pull request with the changes.
