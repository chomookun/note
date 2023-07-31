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


## Updates Certificate

### create manual-auth-hook script

```shell
#!/bin/bash
# godaddy-auth-hook-script.sh

# Set GoDaddy API credentials
export GD_Key="YOUR_GODADDY_API_KEY"
export GD_Secret="YOUR_GODADDY_API_SECRET"

# Extract the domain name from the command-line argument
DOMAIN="$1"

# Extract the DNS challenge details from the environment variables
TXT_NAME="_acme-challenge.${DOMAIN}."
TXT_VALUE="$CERTBOT_VALIDATION"

# Call the GoDaddy API to add the DNS TXT record
curl -X PUT -H "Authorization: sso-key ${GD_Key}:${GD_Secret}" \
  -H "Content-Type: application/json" \
  -d "[{\"name\": \"${TXT_NAME}\", \"type\": \"TXT\", \"data\": \"${TXT_VALUE}\", \"ttl\": 600}] " \
  "https://api.godaddy.com/v1/domains/${DOMAIN}/records/TXT/${TXT_NAME}"
```

## executes certbot command
```shell
certbot certonly --manual --manual-auth-hook /path/godaddy-auth-hook-script.sh --preferred-challenges dns -d domain.com
```










```shell
# dry run
sudo certbot renew --dry-run 

# execute
sudo certbot renew --cert-name "domain.com"
```

