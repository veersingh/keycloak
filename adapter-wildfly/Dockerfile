FROM jboss/wildfly:10.1.0.Final

ENV KEYCLOAK_VERSION 4.0.0.Final

WORKDIR /opt/jboss/wildfly

RUN curl -L https://downloads.jboss.org/keycloak/$KEYCLOAK_VERSION/adapters/keycloak-oidc/keycloak-wildfly-adapter-dist-$KEYCLOAK_VERSION.tar.gz | tar zx
RUN curl -L https://downloads.jboss.org/keycloak/$KEYCLOAK_VERSION/adapters/saml/keycloak-saml-wildfly-adapter-dist-$KEYCLOAK_VERSION.tar.gz | tar zx

WORKDIR /opt/jboss

# Standalone.xml modifications.
# setup SSL
USER root
ADD keycloak.jks $JBOSS_HOME/standalone/configuration/ 
RUN chown jboss:jboss $JBOSS_HOME/standalone/configuration/keycloak.jks
USER jboss
RUN sed -i -e 's/<security-realms>/&\n            <security-realm name="UndertowRealm">\n                <server-identities>\n                    <ssl>\n                        <keystore path="keycloak.jks" relative-to="jboss.server.config.dir" keystore-password="secret" \/>\n                    <\/ssl>\n                <\/server-identities>\n            <\/security-realm>/' $JBOSS_HOME/standalone/configuration/standalone.xml
RUN sed -i -e 's/<server name="default-server">/&\n                <https-listener name="https" socket-binding="https" security-realm="UndertowRealm"\/>/' $JBOSS_HOME/standalone/configuration/standalone.xml

EXPOSE 8080

USER root
