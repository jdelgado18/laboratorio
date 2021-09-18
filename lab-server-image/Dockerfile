# Configuración base de servidor
FROM ibmcom/websphere-liberty:kernel-java8-openj9-ubi
RUN /opt/ibm/wlp/bin/installUtility install localConnector-1.0 ldapRegistry-3.0 appSecurity-3.0
COPY --chown=1001:0 server.xml /config/server.xml
COPY --chown=1001:0 jvm.options /config/jvm.options
COPY --from=hazelcast/hazelcast --chown=1001:0 /opt/hazelcast/lib/*.jar /opt/ibm/wlp/usr/shared/resources/hazelcast/
COPY --chown=1001:0 configDropins/defaults/keystore.xml /config/configDropins/defaults/keystore.xml
COPY --chown=1001:0 key.p12 /config/resources/security/key.p12
ARG VERBOSE=true
ARG HZ_SESSION_CACHE=client
RUN configure.sh

# Configuración de aplicación
RUN /opt/ibm/wlp/bin/installUtility install ejbLite-3.2 wmqJmsClient-2.0 jsp-2.3 mdb-3.2 jpa-2.2
COPY --chown=1001:0 lib/wmq.jmsra.rar /config/resources/jca/wmq.jmsra.rar
COPY --chown=1001:0 lib/db2jcc4.jar /config/resources/lib/db2jcc4.jar
COPY --chown=1001:0 lib/db2jcc_license_cu.jar /config/resources/lib/db2jcc_license_cu.jar
COPY --chown=1001:0 dropins/laboratorio.ear /config/dropins/laboratorio.ear
COPY --chown=1001:0 configDropins/overrides/enterpriseApplication.xml /config/configDropins/overrides/enterpriseApplication.xml
COPY --chown=1001:0 configDropins/overrides/features.xml /config/configDropins/overrides/features.xml
COPY --chown=1001:0 configDropins/overrides/jdbc.xml /config/configDropins/overrides/jdbc.xml
COPY --chown=1001:0 configDropins/overrides/jms.xml /config/configDropins/overrides/jms.xml