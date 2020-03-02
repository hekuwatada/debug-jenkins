# Running Jenkins with Jmx

## Running Jenkins with Docker directly
`docker run -p 8080:8080 --name=jenkins-master -d jenkins/jenkins`

`docker run -p 8080:8080 -p 9010:9010 --name=jenkins-master -d --env JAVA_OPTS="-Dcom.sun.management.jmxremote.rmi.port=9010 -Dcom.sun.management.jmxremote=true -Dcom.sun.management.jmxremote.port=9010 -Dcom.sun.management.jmxremote.ssl=false -Dcom.sun.management.jmxremote.authenticate=false -Dcom.sun.management.jmxremote.local.only=false -Djava.rmi.server.hostname=0.0.0.0" jenkins/jenkins`

## Note
Get container IP
```sh
docker inspect jenkins-master -f '{{.Name}} - {{.NetworkSettings.IPAddress}}'
```
```sh
docker inspect jenkins-master -f '{{.Name}} - {{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}'
```

