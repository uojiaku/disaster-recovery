# disaster-recovery

## Backing Up ETCD data

*It is only necessary to take a backup from a single master host, since each master host share the same data.*

The following procedure generates a single file that contains the ETCD snapshot and static k8s API server resources

### Procedure

Step 1. Access a master host

Step 2. If the cluster-wide proxy is enabled, be sure that you have exported the NO_PROXY, HTTP_PROXY, and HTTPS_PROXY environment variables.
**you can check if the proxy is enabled if the fields ```httpProxy```, ```httpsProxy```, ```noProxy``` are set after running this command ```oc get proxy cluster -o yaml```**

Step 3. Run the ```etcd-snapshot-backup.sh``` script and make sure to pass in the location as to where you want the backup to save to. Here is the appropriate command to run: ```sudo -E /usr/local/bin/etcd-snapshot-backup.sh ./assets/backup```.
*Two files will be generated in ```./assets/backup```  from the above command*
*One file is the ```snapshot_<datetimestamp>.db```: This file is the etcd snapshot*
*The other file is the ```static_kuberesources_<datetimestamp>.tar.gz``` This file contains the static Kubernetes API server resources. If etcd encryption is enabled, it also contains the encryption keys for the etcd snapshot.*


## Replacing a failed master host


## Recovering from lost master host

## Restoring to previous cluster state

## Recovering from expired control plane certificates
