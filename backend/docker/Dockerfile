FROM quay.io/keycloak/keycloak:20.0 as builder
# Add user for scanning Aquesec. Remove if using locally.
USER nobody
WORKDIR /opt/keycloak
# for demonstration purposes only, please make sure to use proper certificates in production instead
RUN keytool -genkeypair -storepass password -storetype PKCS12 -keyalg RSA -keysize 2048 -dname "CN=server" -alias server -ext "SAN:c=DNS:localhost,IP:127.0.0.1" -keystore conf/server.keystore
RUN /opt/keycloak/bin/kc.sh build

FROM quay.io/keycloak/keycloak:20.0

COPY --from=builder /opt/keycloak/ /opt/keycloak/

# Copy themes for NISE
COPY keycloak/themes/ /opt/keycloak/themes/

# copy realms for NISE
COPY keycloak/*.json /opt/keycloak/data/import/

ENTRYPOINT ["/opt/keycloak/bin/kc.sh"]
