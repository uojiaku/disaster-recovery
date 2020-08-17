# Disaster-Recovery

## Backing Up ETCD data

*It is only necessary to take a backup from a single master host, since each master host share the same data.*

The following procedure generates a single file that contains the ETCD snapshot and static k8s API server resources

### Procedure

Step 1. Access a master host

Step 2. If the cluster-wide proxy is enabled, be sure that you have exported the NO_PROXY, HTTP_PROXY, and HTTPS_PROXY environment variables.

*you can check if the proxy is enabled if the fields ```httpProxy```, ```httpsProxy```, ```noProxy``` are set after running this command ```oc get proxy cluster -o yaml```.*

Step 3. Run the ```etcd-snapshot-backup.sh``` script and make sure to pass in the location as to where you want the backup to save to. Here is the appropriate command to run: ```sudo -E /usr/local/bin/etcd-snapshot-backup.sh ./assets/backup```.

*Two files will be generated in ```./assets/backup```  from the above command*

*One file is the ```snapshot_<datetimestamp>.db```: This file is the etcd snapshot*

*The other file is the ```static_kuberesources_<datetimestamp>.tar.gz``` This file contains the static Kubernetes API server resources. If etcd encryption is enabled, it also contains the encryption keys for the etcd snapshot.*


## Replacing a failed master host

### Procedure

To replace a single master host:

#### I. Remove the member from the etcd cluster.

Prerequisites:

- access to the cluster as a user with the ```cluster-admin``` role.

- SSH access to an active master host.

Procedure:

1. View pods associated with ETCD.

    ```oc get pods -n openshift-etcd```

2. Access an active master host.

3. Run the ```etcd-member-remove.sh``` script and pass in the name of the ETCD member to remove.

   ```sudo -E /usr/local/bin/etcd-member-remove.sh etcd-member-ip-10-x-x-x.subdomain.domain```

4. Verify that the etcd member has been successfully removed from the cluster.

   a. Connect to the running etcd container.

      ```id=$(sudo crictl ps --name etcd-member | awk 'FNR==2{ print $1}') && sudo crictl exec -it $id /bin/sh```

   b. In the etcd container, export the variables needed for connecting to etcd.

      ```export ETCDCTL_API=3 ETCDCTL_CACERT=/etc/ssl/etcd/ca.crt ETCDCTL_CERT=$(find /etc/ssl/ -name *peer*crt) ETCDCTL_KEY=$(find /etc/ssl/ -name *peer*key)```

   c. In the etcd container, execute etcdctl member list and verify that the removed member is no longer listed:

      ```etcdctl member list -w table```

#### II. If the etcd certificates for the master host are valid, then add the member back to the etcd cluster.

Prerequisites:

- access to cluster as a user with ```cluster-admin``` role.

- SSH access to master host to add to the etcd cluster.

- Obtain the IP address of an existing active etcd member.

Procedure:

1. Access the master host to add to the etcd cluster.

2. Run the ```etcd-member-add.sh``` script and pass in two parameters:

- IP address of an existing etcd member

- the name of the etcd member to add 

Here's an example below:

![](/images/etcd-member-add.png)

![](/images/etcd-member-add-notes.png)


#### III. If there are no etcd certificates for the master host or they are no longer.


## Recovering from lost master host

## Restoring to previous cluster state

## Recovering from expired control plane certificates
