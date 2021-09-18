# PRE Configuración base de servidor
FROM ibmcom/websphere-liberty:kernel-java8-openj9-ubi
RUN /opt/ibm/wlp/bin/installUtility install localConnector-1.0 ldapRegistry-3.0 sessioncache-1.0
# COPY --chown=1001:0 lib/hazelcast-4.2.2.jar /opt/ibm/wlp/usr/shared/resources/hazelcast/hazelcast.jar
# COPY --from=hazelcast/hazelcast --chown=1001:0 /opt/hazelcast/lib/cache-api-1.1.1.jar /opt/ibm/wlp/usr/shared/resources/hazelcast/cache-api-1.1.1.jar
COPY --chown=1001:0 server.xml /config/server.xml
COPY --chown=1001:0 jvm.options /config/jvm.options
COPY --chown=1001:0 configDropins/defaults/keystore.xml /config/configDropins/defaults/keystore.xml
COPY --chown=1001:0 key.p12 /config/resources/security/key.p12
# COPY --chown=1001:0 configDropins/overrides/hazelcast-sessioncache.xml /config/configDropins/overrides/hazelcast-sessioncache.xml
# COPY --chown=1001:0 shared/hazelcast.xml /opt/ibm/wlp/usr/shared/config/hazelcast/hazelcast.xml

# Configuración de aplicación
RUN /opt/ibm/wlp/bin/installUtility install ejbLite-3.2 wmqJmsClient-2.0 jsp-2.3 mdb-3.2 jpa-2.2 appSecurity-3.0
COPY --chown=1001:0 lib/wmq.jmsra.rar /config/resources/jca/wmq.jmsra.rar
COPY --chown=1001:0 lib/db2jcc4.jar /config/resources/lib/db2jcc4.jar
COPY --chown=1001:0 lib/db2jcc_license_cu.jar /config/resources/lib/db2jcc_license_cu.jar
COPY --chown=1001:0 dropins/laboratorio.ear /config/apps/laboratorio.ear
COPY --chown=1001:0 configDropins/overrides/enterpriseApplication.xml /config/configDropins/overrides/enterpriseApplication.xml
COPY --chown=1001:0 configDropins/overrides/features.xml /config/configDropins/overrides/features.xml
COPY --chown=1001:0 configDropins/overrides/jdbc.xml /config/configDropins/overrides/jdbc.xml
COPY --chown=1001:0 configDropins/overrides/jms.xml /config/configDropins/overrides/jms.xml

### Hazelcast Session Caching ###
# Copy the Hazelcast libraries from the Hazelcast Docker image
COPY --from=hazelcast/hazelcast --chown=1001:0 /opt/hazelcast/lib/*.jar /opt/ibm/wlp/usr/shared/resources/hazelcast/

# Instruct configure.sh to copy the client topology hazelcast.xml
# ARG HZ_SESSION_CACHE=client

# Instruct configure.sh to copy the embedded topology hazelcast.xml and set the required system property
ARG HZ_SESSION_CACHE=embedded
ENV JAVA_TOOL_OPTIONS="-Dhazelcast.jcache.provider.type=server ${JAVA_TOOL_OPTIONS}"

## This script will add the requested XML snippets and grow image to be fit-for-purpose
RUN configure.sh