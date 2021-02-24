# OpenShift Container Storage

## OCS Nodes Labels and Taints
### Infra Role
To label OCS nodes as infra nodes:
```
oc label <node_name> node-role.kubernetes.io/infra=""
```
This command updates the following configuration in the node resource:
```
labels:
  node-role.kubernetes.io/infra: ""
  node-role.kubernetes.io/worker: ""
```

**Do not remove the worker role label. The MCO does not support managing nodes
not labeled as master or worker.**

### 
Add taints to avoid scheduling of non OCS nodes.
```
oc adm taint node <node_name> node.ocs.openshift.io/storage="true":NoSchedule
```

This command updates the following configuration in the node resource:
```
spec:
  taints:
  - effect: NoSchedule
    key: node.ocs.openshift.io/storage
    value: "true"
```


## Examples
Examples related to OpenShift Container Storage.

- **Ceph Toolbox**. The Ceph Toolbox is maintained under the Rook project and is an
  unsupported feature of OCS. 
  More info [here](https://github.com/rook/rook/blob/master/Documentation/ceph-toolbox.md).
  
  To enable the toolbox in an OCS cluster edit the resource `ocsinitializations.ocs.openshift.io/ocsinit`:
  ```
  oc patch OCSInitialization ocsinit -n openshift-storage --type json --patch '[{ "op": "replace", "path": "/spec/enableCephTools", "value": true }]'
  ```
  
  **WARNING** The Ceph Tools pod is meant to be a troubleshooting tool only. The 
  Ceph cluster is managed by the OCS operator only and no modifications to the 
  cluster configuration, crush maps, OSDs should be applied using command line tools.
  This feature is not supported under OCS and should be used for troubleshooting and inspection only.

  Alternatively, a standalone deployment can be found here, customized to match the `openshift-storage` namespace as 
  well as the tolerations to match OCS nodes taints.


  Access the toolbox pod:  
  ```
  $ oc rsh rook-ceph-tools-<id>
  ```
  ### Troubleshooting examples:

  View cluster status:
  ```
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

  List objects in rbd pool:
  ```
  $ rados -p ocs-storagecluster-cephblockpool ls
  ```

  List objects in cephfs data pool:
  ```
  $ rados -p ocs-storagecluster-cephfilesystem-data0 ls
  ```

  View used storage space:
  ```
  $ rados df
  ```

  View used storage with a tree like formatting (more info about OCS storage nodes):
  ```
  $ ceph osd df tree
  ```

  View OSDs performances (commit_latency, apply_latency) in millesconds:
  ```
  $ ceph osd perf
  ```

  View Placement Groups stats:
  ```
  $ ceph pg stat
  ```

  Dump CRUSH map:
  ```
  $ ceph osd crush dump
  ```

  Dump CephX accounts keys and caps:
  ```
  $ ceph auth list
  ```

  View I/O stats:
  ```
  $ ceph iostat
  ```
  
