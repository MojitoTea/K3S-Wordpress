# K3S-Wordpress
# Setting up Kubernetes

Tested Environment:
- Ubuntu 20.04
- 12GB RAM
- 8 CPU
- 40 GB Disk space, 2GB Swap

#K3S Installing
https://docs.k3s.io/quick-start

# Deploying WordPress Example

Now we can Deploy (modified) WordPress example from:
- https://kubernetes.io/docs/tutorials/stateful-application/mysql-wordpress-persistent-volume/

Setup directory for YAML manifests:
```bash
git clone https://github.com/MojitoTea/K3S-Wordpress
```

Try dry-run mode to see if YAML files are correct:
```bash
kubectl apply -k ./ --dry-run
```

Now create our custom namespace and run custom.yaml:
```bash
kubectl create namespace wp1
kubectl apply -k ./
```

Now poll this command:
```bash
kubectl get pods -n wp1
```
And wait till all Pods are in state `Running`, for example:
```bash
$ kubectl get pods -n wp1

NAME                               READY   STATUS    RESTARTS   AGE
wordpress-mysql-1jk9d239bd-lpxjf   1/1     Running   0          10m
wordpress-12h53b758-4kig4          1/1     Running   0          10m
```

 WP IP address and port:
```bash
$ kubectl get svc wordpress -n wp1

NAME        TYPE       CLUSTER-IP      EXTERNAL-IP   PORT(S)        AGE
wordpress   NodePort   10.42.211.142   <none>        80:31258/TCP   16m
```

Application requests them via PVC - Persistent Volume Claims. In our example
you can see them with:
```bash
$ kubectl get pvc -n wp1

NAME             STATUS   VOLUME                                     CAPACITY   ACCESS MODES   STORAGECLASS   AGE
wp-pv-claim      Bound    pvc-5856h332-8432-4723-b986-a64769hjk88f   20Gi       RWO            local-path     37m
mysql-pv-claim   Bound    pvc-8378jbdf-43h4-4311-84j2-f7896jg77eae   20Gi       RWO            local-path     37m
```
