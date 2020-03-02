# Running Jenkins with Jmx

## Running Jenkins with Docker locally
`docker run -p 8080:8080 --name=jenkins-master -d jenkins/jenkins`

`docker run -p 8080:8080 -p 9010:9010 --name=jenkins-master -d --env JAVA_OPTS="-Dcom.sun.management.jmxremote.rmi.port=9010 -Dcom.sun.management.jmxremote=true -Dcom.sun.management.jmxremote.port=9010 -Dcom.sun.management.jmxremote.ssl=false -Dcom.sun.management.jmxremote.authenticate=false -Dcom.sun.management.jmxremote.local.only=false -Djava.rmi.server.hostname=0.0.0.0" jenkins/jenkins`

- `hostname` is the IP address of the host where you run docker image (e.g. localhost, 0.0.0.0)
- `rmi.port` and `jmxremote.port` should be the same

## Running Jenkins on Kubernetes

Kubernetes version used
```sh
% kubectl version
Client Version: version.Info{Major:"1", Minor:"14"
Server Version: version.Info{Major:"1", Minor:"17"
```
### Deploy Jenkins master
1. Create a namespace
```sh
kubectl create ns jenkins-master
```
2. Deploy jenkins
```sh
kubectl -n jenkins-master apply -f  k8s/jenkins-deployment.yaml
```
3. Get the running jenkins pod name
```sh
kubectl -n jenkins-master get pod,deployment,replicaset -o wide --show-labels
```

### Access Jmx port with Kubernetes Port forwarding
Port-forward the jenkins pod
```sh
kubectl -n jenkins-master port-forward <jenkins-master-pod-name> 9010
```

### Access Jmx port with Kubernetes Service
TODO


## Note
Get container IP
```sh
docker inspect jenkins-master -f '{{.Name}} - {{.NetworkSettings.IPAddress}}'
```
```sh
docker inspect jenkins-master -f '{{.Name}} - {{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}'
```

