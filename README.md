# disaster-recovery

## Backing Up ETCD data

*It is only necessary to take a backup from a single master host, since each master host share the same data.*

The following procedure generates a single file that contains the ETCD snapshot and static k8s API server resources

### Procedure

Step 1. Access a master host

Step 2. If the cluster-wide proxy is enabled, be sure that you have exported the NO_PROXY, HTTP_PROXY, and HTTPS_PROXY environment variables.
*you can check if the proxy is enabled if the fields ```httpProxy```, ```httpsProxy```, ```noProxy``` are set after running this command ```oc get proxy cluster -o yaml```*


## Replacing a failed master host


## Recovering from lost master host

## Restoring to previous cluster state

## Recovering from expired control plane certificates
