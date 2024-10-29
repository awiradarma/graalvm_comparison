## Steps to build OpenLiberty container with InstantOn using Docker

Build the project (generate the WAR file)
```
./mvnw package
```

Build the container image
```
docker build -t openliberty .
```

Run the container image in privileged mode and take a checkpoint
```
docker run --name checkpoint --privileged --env WLP_CHECKPOINT=afterAppStart openliberty
```

Save the stopped container image as a new container image
```
docker commit checkpoint liberty-instanton
```

Remove the stopped container process
```
docker rm checkpoint
```

Run the instanton image in privileged mode
```
docker run -p 9080:9080 --name instant -d --privileged liberty-instanton

[AUDIT   ] Launching defaultServer (Open Liberty 24.0.0.10/wlp-1.0.94.cl241020240923-1638) on Eclipse OpenJ9 VM, version 21.0.4+7-LTS (en_US)
[AUDIT   ] CWWKT0016I: Web application available (default_host): http://3bb0c024b0a3:9080/metrics/
[AUDIT   ] CWWKT0016I: Web application available (default_host): http://3bb0c024b0a3:9080/liberty/
[AUDIT   ] CWWKT0016I: Web application available (default_host): http://3bb0c024b0a3:9080/ibm/api/
[AUDIT   ] CWWKC0452I: The Liberty server process resumed operation from a checkpoint in 0.265 seconds.
[AUDIT   ] CWWKZ0001I: Application liberty started in 0.268 seconds.
[AUDIT   ] CWWKF0012I: The server installed the following features: [cdi-4.0, distributedMap-1.0, jndi-1.0, json-1.0, jsonb-3.0, jsonp-2.1, monitor-1.0, mpConfig-3.1, mpMetrics-5.1, restfulWS-3.1, restfulWSClient-3.1, ssl-1.0, transportSecurity-1.0].
[AUDIT   ] CWWKF0011I: The defaultServer server is ready to run a smarter planet. The defaultServer server started in 3.393 seconds.
```