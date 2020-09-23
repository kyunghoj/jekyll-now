# CephFS

## Configuring Directory Fragmentation
https://docs.ceph.com/docs/mimic/cephfs/dirfrags/

In CephFS, directories are *fragmented* when they become very large or very busy. This splits up the metadata so that it can be shared between multiple MDS daemons, and between multiple objects in the metadata pool.

All directories are initially created as a single fragment. This fragment may be split to divide up the directory into more fragments, and these fragments may be *merged* to reduce the number of gragments in the directory.

### Splitting and merging

When an MDS identifies a directory fragment to be split, it does not do the split immediately. Because splitting interrupts metadata IO, a short delay is used to allow short bursts of client IO to complete before the split begins. This delay is configured with mds_bal_fragment_interval, which defaults to 5 seconds.

When the split is done, the directory fragment is broken up into a power of two number of new fragments. The number of new fragments is given by two to the power mds_bal_split_bits, i.e. if mds_bal_split_bits is 2, then four new fragments will be created. The default setting is 3, i.e. splits create 8 new fragments.

The criteria for initiating a split or a merge are described in the following sections.

### Size Thresholds

