FROM openjdk:8-alpine

ENV SONAR_VERSION=6.7.6 \
    SONARQUBE_HOME=/opt/sonarqube \
    # Database configuration
    # Defaults to using H2
    SONARQUBE_JDBC_USERNAME=sonar \
    SONARQUBE_JDBC_PASSWORD=sonar \
    SONARQUBE_JDBC_URL=

# Http port
EXPOSE 9000

RUN addgroup -S sonarqube \
    && adduser -S -G sonarqube sonarqube

# pub   2048R/D26468DE 2015-05-25
#       Key fingerprint = F118 2E81 C792 9289 21DB  CAB4 CFCA 4A29 D264 68DE
# uid                  sonarsource_deployer (Sonarsource Deployer) <infra@sonarsource.com>
# sub   2048R/06855C1D 2015-05-25
RUN set -x \
    && apk add --no-cache gnupg unzip libressl curl bash \
    && gpg --keyserver keyserver.ubuntu.com --recv-keys F1182E81C792928921DBCAB4CFCA4A29D26468DE \
    && mkdir -p /opt \
    && cd /opt \
    && wget -O sonarqube.zip --no-verbose https://binaries.sonarsource.com/Distribution/sonarqube/sonarqube-$SONAR_VERSION.zip \
    && wget -O sonarqube.zip.asc --no-verbose https://binaries.sonarsource.com/Distribution/sonarqube/sonarqube-$SONAR_VERSION.zip.asc \
    && gpg --batch --verify sonarqube.zip.asc sonarqube.zip \
    && unzip sonarqube.zip \
    && mv sonarqube-$SONAR_VERSION sonarqube \
    && chown -R sonarqube:sonarqube sonarqube \
    && rm sonarqube.zip* \
    && rm -rf $SONARQUBE_HOME/bin/*

VOLUME "$SONARQUBE_HOME/data"
WORKDIR $SONARQUBE_HOME
COPY run.sh $SONARQUBE_HOME/bin/
USER sonarqube
ENTRYPOINT ["./bin/run.sh"]
