# Network Policy examples

## Environment preparation
Create example namespace
```
$ oc new-project example-project
```

Create example deployment, service and route
```
$ oc create deployment hello-openshift --image=quay.io/gbsalinetti/hello-openshift --port 8080 -n example-project
$ oc expose deployment hello-openshift -n example-project
$ oc expose svc hello-openshift -n example-project
```

## Network Policy creation
```
$ oc apply -f <network_policy_file>
```

## Testing
[Open a new tab/window]
(Optional) Create new test project 
```
$ oc new-project test-project
```

Create a client image to perform connectivity tests.
```
$ kubectl run --rm -i -t --image=registry.access.redhat.com/ubi8/ubi-minimal test-client -- sh
```

## Cleanup
```
$ oc delete deployment/hello-openshift -n example-project
$ oc delete svc/hello-openshift -n example-project
$ oc delete route/hello-openshift -n example-project
$ oc delete NetworkPolicy/<name> -n example-project
```
