# Installation

wget https://github.com/keycloak/keycloak/releases/download/20.0.1/keycloak-20.0.1.zip
unzip ./keycloak*.zip
ln -s ./keycloak-20.0.1 ./keycloak


# Install postgresql

sudo apt-get install postgresql postgresql-client
sudo su - postgres
psql --verison

# Creates user


