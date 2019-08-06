---
title: Linux性能优化-实验报告23
date: 2019-08-06 17:58:19
tags: [Perf]
---

最后留的问题做的实验。看的下面的评论，我自己使用的bcc cachetop，但看结果毫无变化

```
# cachetop #使用了echo 3 >/proc/sys/vm/drop_caches，但结果一直是下面的情况
09:50:57 Buffers MB: 6202 / Cached MB: 386 / Sort: HITS / Order: ascending
```


```
root@perf:/home/ykg# echo 3 >/proc/sys/vm/drop_caches
root@perf:/home/ykg# slabtop -o -s c
 Active / Total Objects (% used)    : 386384 / 421424 (91.7%)
 Active / Total Slabs (% used)      : 7718 / 7718 (100.0%)
 Active / Total Caches (% used)     : 100 / 142 (70.4%)
 Active / Total Size (% used)       : 104914.02K / 113871.35K (92.1%)
 Minimum / Average / Maximum Object : 0.01K / 0.27K / 8.00K

  OBJS ACTIVE  USE OBJ SIZE  SLABS OBJ/SLAB CACHE SIZE NAME
 32192  32192 100%    0.50K    503       64     16096K kmalloc-512
 25917  23940   0%    0.59K    489       53     15648K inode_cache
 76740  74058   0%    0.13K   1279       60     10232K kernfs_node_cache
 41244  31900   0%    0.19K    982       42      7856K dentry
   600    587   0%    8.00K    150        4      4800K kmalloc-8192
  7616   6548   0%    0.57K    136       56      4352K radix_tree_node
 16640  14915   0%    0.25K    260       64      4160K filp
  3780   2743   0%    1.06K    126       30      4032K ext4_inode_cache
   625    528   0%    5.75K    125        5      4000K task_struct
  3200   2991   0%    1.00K    100       32      3200K kmalloc-1024
 15249  14950   0%    0.20K    391       39      3128K vm_area_struct
  4560   3949   0%    0.66K     95       48      3040K proc_inode_cache
  1344   1316   0%    2.00K     84       16      2688K kmalloc-2048
   592    559   0%    4.00K     74        8      2368K kmalloc-4096
  2806   2761   0%    0.69K     61       46      1952K sock_inode_cache
  2714   2422   0%    0.70K     59       46      1888K shmem_inode_cache
  1664   1664 100%    1.00K     52       32      1664K UNIX

root@perf:/home/ykg# find / -name file-name
root@perf:/home/ykg# slabtop -o -s c
 Active / Total Objects (% used)    : 577221 / 601088 (96.0%)
 Active / Total Slabs (% used)      : 11750 / 11750 (100.0%)
 Active / Total Caches (% used)     : 100 / 142 (70.4%)
 Active / Total Size (% used)       : 173855.90K / 181403.00K (95.8%)
 Minimum / Average / Maximum Object : 0.01K / 0.30K / 8.00K

  OBJS ACTIVE  USE OBJ SIZE  SLABS OBJ/SLAB CACHE SIZE NAME
 25770  24847   0%    1.06K    859       30     27488K ext4_inode_cache
 35881  34367   0%    0.59K    677       53     21664K inode_cache
 31296  30755   0%    0.66K    652       48     20864K proc_inode_cache
106050  97548   0%    0.19K   2525       42     20200K dentry
 32192  32192 100%    0.50K    503       64     16096K kmalloc-512
 76740  74058   0%    0.13K   1279       60     10232K kernfs_node_cache
  6591   6396   0%    0.81K    169       39      5408K fuse_inode
   600    587   0%    8.00K    150        4      4800K kmalloc-8192
  7896   6828   0%    0.57K    141       56      4512K radix_tree_node
 16768  15224   0%    0.25K    262       64      4192K filp
   625    528   0%    5.75K    125        5      4000K task_struct
 31395  31395 100%    0.10K    805       39      3220K buffer_head
  3200   2991   0%    1.00K    100       32      3200K kmalloc-1024
 15366  15067   0%    0.20K    394       39      3152K vm_area_struct
  1344   1316   0%    2.00K     84       16      2688K kmalloc-2048
   592    559   0%    4.00K     74        8      2368K kmalloc-4096
  2806   2761   0%    0.69K     61       46      1952K sock_inode_cache
```
