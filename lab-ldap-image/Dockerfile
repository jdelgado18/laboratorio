FROM openjdk:8u111-jre-alpine

EXPOSE 389 636

RUN apk add --no-cache openssl
RUN mkdir /ldap
WORKDIR /ldap
RUN wget https://github.com/kwart/ldap-server/releases/download/v.1.0.0/ldap-server.jar

COPY usuarios.ldif .
COPY key.jks .

CMD ["java","-jar","ldap-server.jar", "-p", "389", "-sp", "636", "-skf", "key.jks", "-skp", "passw0rd", "usuarios.ldif"]