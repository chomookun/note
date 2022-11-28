# download
wget -e use-proxy=yes -e http_proxy=http://10.11.100.7:8888 -e https_proxy=https://10.11.100.7:8888 \
https://github.com/keycloak/keycloak/releases/download/15.1.1/keycloak-15.1.1.zip

# add admin
./add-user-keycloak.sh -u admin -p rhksflwk

# start
./standalone.sh -Djboss.management.http.port=8180 -b 0.0.0.0 -bmanagement 0.0.0.0


