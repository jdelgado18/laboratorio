FROM ibmcom/websphere-liberty:kernel-java8-openj9-ubi
RUN /opt/ibm/wlp/bin/installUtility install localConnector-1.0 ejbLite-3.2 ldapRegistry-3.0 wmqJmsClient-2.0 jsp-2.3 appSecurity-3.0 mdb-3.2 jpa-2.2
COPY --chown=1001:0 lib/wmq.jmsra.rar /config/resources/jca/wmq.jmsra.rar
COPY --chown=1001:0 lib/db2jcc4.jar /config/resources/lib/db2jcc4.jar
COPY --chown=1001:0 lib/db2jcc_license_cu.jar /config/resources/lib/db2jcc_license_cu.jar
COPY --chown=1001:0 src/key.p12 /opt/ibm/wlp/output/defaultServer/resources/security/key.p12
COPY --chown=1001:0 src/server.xml /config/server.xml
COPY --chown=1001:0 src/laboratorio.ear /config/apps/laboratorio.ear