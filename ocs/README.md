# OpenShift Container Storage

Examples related to OpenShift Container Storage.

- **Ceph Toolbox**. The Ceph Toolbox maintained under the Rook project. More info
  [here](https://github.com/rook/rook/blob/master/Documentation/ceph-toolbox.md).
  The deployment was customized to match the `openshift-storage` namespace as 
  well as the tolerations to match OCS nodes taints.

  Examples:
  ```
  $ oc rsh rook-ceph-tools-<id>
  $ ceph -s
    cluster:
    id:     c03d7d1d-b0a2-413f-b8b5-4f26b397e1ce
    health: HEALTH_OK

  services:
    mon: 3 daemons, quorum a,b,c (age 10h)
    mgr: a(active, since 10h)
    mds: ocs-storagecluster-cephfilesystem:1 {0=ocs-storagecluster-cephfilesystem-b=up:active} 1 up:standby-replay
    osd: 3 osds: 3 up (since 10h), 3 in (since 10h)

  task status:
    scrub status:
        mds.ocs-storagecluster-cephfilesystem-a: idle
        mds.ocs-storagecluster-cephfilesystem-b: idle

  data:
    pools:   3 pools, 96 pgs
    objects: 277 objects, 755 MiB
    usage:   5.0 GiB used, 1.5 TiB / 1.5 TiB avail
    pgs:     96 active+clean

  io:
    client:   1.2 KiB/s rd, 5.7 KiB/s wr, 2 op/s rd, 0 op/s wr
  ```
  
  **WARNING** The Ceph Tools pod is meant to be a troubleshooting tool only. The 
  Ceph cluster is managed by the OCS operator only and no modifications to the 
  cluster configuration, crush maps, OSDs should be applied using command line tools.
