---
title: MacでTimemachine用にAPFS Volumeを作り直す
date: 2021-01-04 14:16:48
tags:
---


## 実行前の状態
- disk4が外付けの3TBストレージ　ここの一部に、APFS Volumeを作りたい
$ diskutil list

```
/dev/disk0 (internal):
   #:                       TYPE NAME                    SIZE       IDENTIFIER
   0:      GUID_partition_scheme                         251.0 GB   disk0
   1:             Apple_APFS_ISC ⁨⁩                        524.3 MB   disk0s1
   2:                 Apple_APFS ⁨Container disk3⁩         245.1 GB   disk0s2
   3:        Apple_APFS_Recovery ⁨⁩                        5.4 GB     disk0s3

/dev/disk3 (synthesized):
   #:                       TYPE NAME                    SIZE       IDENTIFIER
   0:      APFS Container Scheme -                      +245.1 GB   disk3
                                 Physical Store disk0s2
   1:                APFS Volume ⁨Macintosh HD⁩            15.1 GB    disk3s1
   2:              APFS Snapshot ⁨com.apple.os.update-...⁩ 15.1 GB    disk3s1s1
   3:                APFS Volume ⁨Preboot⁩                 303.0 MB   disk3s2
   4:                APFS Volume ⁨Recovery⁩                1.1 GB     disk3s3
   5:                APFS Volume ⁨Data⁩                    61.8 GB    disk3s5
   6:                APFS Volume ⁨VM⁩                      3.2 GB     disk3s6

/dev/disk4 (external, physical):
   #:                       TYPE NAME                    SIZE       IDENTIFIER
   0:      GUID_partition_scheme                        *3.0 TB     disk4
   1:                        EFI ⁨EFI⁩                     209.7 MB   disk4s1
   2:                 Apple_APFS ⁨Container disk5⁩         750.1 GB   disk4s2
   3:       Microsoft Basic Data ⁨ExternalDat⁩             1.5 TB     disk4s3
   4:                 Apple_APFS ⁨Container disk6⁩         750.1 GB   disk4s4

/dev/disk5 (synthesized):
   #:                       TYPE NAME                    SIZE       IDENTIFIER
   0:      APFS Container Scheme -                      +750.1 GB   disk5
                                 Physical Store disk4s2
   1:                APFS Volume ⁨TimeMachine⁩             1.2 MB     disk5s1

/dev/disk6 (synthesized):
   #:                       TYPE NAME                    SIZE       IDENTIFIER
   0:      APFS Container Scheme -                      +750.1 GB   disk6
                                 Physical Store disk4s4
   1:                APFS Volume ⁨a⁩                       1.2 MB     disk6s1
```


## コマンド

yusuke@yusukenoMacBook-Air-->/Users/yusuke:::
$ diskutil apfs deleteContainer disk4s4
Started APFS operation on disk6
Deleting APFS Container with all of its APFS Volumes
Unmounting Volumes
Unmounting Volume "a" on disk6s1
Deleting Volumes
Deleting Container
Wiping former APFS disks
Switching content types
1 new disk created or changed due to APFS operation
Disk from APFS operation: disk4s4
Finished APFS operation on disk6
Removing disk4s4 from partition map



## コマンド実行後

$ diskutil list
/dev/disk0 (internal):
   #:                       TYPE NAME                    SIZE       IDENTIFIER
   0:      GUID_partition_scheme                         251.0 GB   disk0
   1:             Apple_APFS_ISC ⁨⁩                        524.3 MB   disk0s1
   2:                 Apple_APFS ⁨Container disk3⁩         245.1 GB   disk0s2
   3:        Apple_APFS_Recovery ⁨⁩                        5.4 GB     disk0s3

/dev/disk3 (synthesized):
   #:                       TYPE NAME                    SIZE       IDENTIFIER
   0:      APFS Container Scheme -                      +245.1 GB   disk3
                                 Physical Store disk0s2
   1:                APFS Volume ⁨Macintosh HD⁩            15.1 GB    disk3s1
   2:              APFS Snapshot ⁨com.apple.os.update-...⁩ 15.1 GB    disk3s1s1
   3:                APFS Volume ⁨Preboot⁩                 303.0 MB   disk3s2
   4:                APFS Volume ⁨Recover                1.1 GB     disk3s3
   5:                APFS Volume ⁨Dat                    61.8 GB    disk3s5
   6:                APFS Volume ⁨V                      3.2 GB     disk3s6

/dev/disk4 (external, physical):
   #:                       TYPE NAME                    SIZE       IDENTIFIER
   0:      GUID_partition_scheme                        *3.0 TB     disk4
   1:                        EFI ⁨EF                     209.7 MB   disk4s1
   2:                 Apple_APFS ⁨Container disk         750.1 GB   disk4s2
   3:       Microsoft Basic Data ⁨ExternalDa             1.5 TB     disk4s3
                    (free space)                         750.2 GB   -

/dev/disk5 (synthesized):
   #:                       TYPE NAME                    SIZE       IDENTIFIER
   0:      APFS Container Scheme -                      +750.1 GB   disk5
                                 Physical Store disk4s2
   1:                APFS Volume ⁨TimeMachin             1.2 MB     disk5s1


## 一度、全消し
$ diskutil apfs deleteContainer disk4s2
Started APFS operation on disk5
Deleting APFS Container with all of its APFS Volumes
Unmounting Volumes
Unmounting Volume "TimeMachine" on disk5s1
Deleting Volumes
Deleting Container
Wiping former APFS disks
Switching content types
1 new disk created or changed due to APFS operation
Disk from APFS operation: disk4s2
Finished APFS operation on disk5
Removing disk4s2 from partition map

## コマンド実行後
$ diskutil list
/dev/disk0 (internal):
   #:                       TYPE NAME                    SIZE       IDENTIFIER
   0:      GUID_partition_scheme                         251.0 GB   disk0
   1:             Apple_APFS_ISC                         524.3 MB   disk0s1
   2:                 Apple_APFS ⁨Container disk         245.1 GB   disk0s2
   3:        Apple_APFS_Recovery                         5.4 GB     disk0s3

/dev/disk3 (synthesized):
   #:                       TYPE NAME                    SIZE       IDENTIFIER
   0:      APFS Container Scheme -                      +245.1 GB   disk3
                                 Physical Store disk0s2
   1:                APFS Volume ⁨Macintosh H            15.1 GB    disk3s1
   2:              APFS Snapshot ⁨com.apple.os.update-.. 15.1 GB    disk3s1s1
   3:                APFS Volume ⁨Preboo                 303.0 MB   disk3s2
   4:                APFS Volume ⁨Recover                1.1 GB     disk3s3
   5:                APFS Volume ⁨Dat                    61.9 GB    disk3s5
   6:                APFS Volume ⁨V                      3.2 GB     disk3s6

/dev/disk4 (external, physical):
   #:                       TYPE NAME                    SIZE       IDENTIFIER
   0:      GUID_partition_scheme                        *3.0 TB     disk4
   1:                        EFI ⁨EF                     209.7 MB   disk4s1
   2:                  Apple_KFS                         750.1 GB   disk4s2
   3:       Microsoft Basic Data ⁨ExternalDa             1.5 TB     disk4s3
                    (free space)                         750.2 GB   -

/dev/disk5 (synthesized):
   #:                       TYPE NAME                    SIZE       IDENTIFIER
   0:      APFS Container Scheme -                      +750.1 GB   disk5
                                 Physical Store disk4s2



## Reference
- [Disks, partitions, volumes, containers – The Eclectic Light Company](https://eclecticlight.co/2018/08/01/disks-partitions-volumes-containers/)