# Network Config Examples

### View Network CRD
```
$ oc get networks.config.openshift.io/cluster -o yaml
```

### View ClusterNetworks CRD
This CRD is managed by the Cluster Network Operator and defines how the SDN
should be configured.
```
$ oc get clusternetworks.network.openshift.io -o yaml
```


### Inspect CNO logs
```
$ oc logs network-operator-<UUID> -n openshift-network-operator
```

The CNO manages the pods running in the `openshift-sdn` namespace.
Management logs and potential errors can be found here.

### View openshift-sdn namespace pods
```
$ oc get pods -n openshift-sdn
```

### Inspect OVS services
The OVS service runs as native systemd unit but a pod on the corresponding
host is created to tail its logs and enabel access to OVS CLI tools for 
debugging and troubleshooting.

Show OVS switches:
```
$ oc exec ovs-5k78b ovs-vsctl show
```

Dump OpenFlow rules on a switch:
```
$ oc exec ovs-5k78b -- ovs-ofctl dump-flows br0 -OOpenFlow13
```

### Inspect SDN logs
Every cluster node runs an SDN pods that implements the CNI interface and
embeds the kube-proxy service.
```
$ oc logs sdn-f7cj9  -c sdn
```

