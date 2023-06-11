# Let's encrypt SSL Certificates

## Install certbot
```shell
sudo apt-get install certbot
```

## Execute create command
```shell
sudo certbot certonly --manual --preferred-challenges dns -d "*.domain.com" -d "domain.com"
```
### create 1st/2nd DNS TXT record

### confirm 1st/2nd DNS TXT 
https://toolbox.googleapps.com/apps/dig/#TXT/_acme-challenge.domain.com


## Merge PEM file
```shell
cd /etc/letsencrypt/live/
cat DOMAIN/fullchain.pem DOMAIN/privkey.pem > ./DOMAIN.pem
```
