# Stage and thin the application 
FROM icr.io/appcafe/open-liberty:full-java21-openj9-ubi-minimal as staging

COPY --chown=1001:0 target/Simple-0.0.1-SNAPSHOT.jar \
                    /staging/Simple-0.0.1-SNAPSHOT.jar

RUN springBootUtility thin \
 --sourceAppPath=/staging/Simple-0.0.1-SNAPSHOT.jar \
 --targetThinAppPath=/staging/thin-Simple-0.0.1-SNAPSHOT.jar \
 --targetLibCachePath=/staging/lib.index.cache

# Build the image
FROM icr.io/appcafe/open-liberty:kernel-slim-java21-openj9-ubi-minimal

ARG VERSION=1.0
ARG REVISION=SNAPSHOT

LABEL \
  org.opencontainers.image.authors="Your Name" \
  org.opencontainers.image.vendor="Open Liberty" \
  org.opencontainers.image.url="local" \
  org.opencontainers.image.source="" \
  org.opencontainers.image.version="$VERSION" \
  org.opencontainers.image.revision="$REVISION" \
  vendor="Open Liberty" \
  name="simple app" \
  version="$VERSION-$REVISION" \
  summary="A simple Spring Boot API" \
  description="This image contains a Spring Boot application running with the Open Liberty runtime."

RUN cp /opt/ol/wlp/templates/servers/springBoot3/server.xml /config/server.xml

RUN features.sh

COPY --chown=1001:0 --from=staging /staging/lib.index.cache /lib.index.cache
COPY --chown=1001:0 --from=staging /staging/thin-Simple-0.0.1-SNAPSHOT.jar \
                    /config/dropins/spring/thin-Simple-0.0.1-SNAPSHOT.jar

RUN configure.sh 
