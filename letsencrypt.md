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
```shall
dig _acme-challenge.{DOMAIN} txt
```

## Merge PEM file
```shell
sudo cat /etc/letsencrypt/live/{DOMAIN}/fullchain.pem /etc/letsencrypt/live/{DOMAIN}/privkey.pem > ./DOMAIN.pem
```


## Updates Certificate

### create manual-auth-hook script

```shell
#!/bin/bash
# godaddy-auth-hook-script.sh

# Set GoDaddy API credentials
GD_Key="_________"
GD_Secret="__________"
DOMAIN="___________"

# Extract the DNS challenge details from the environment variables
TXT_NAME="_acme-challenge"
TXT_VALUE="$CERTBOT_VALIDATION"
echo "DOMAIN:${DOMAIN}"
echo "TXT_NAME:${TXT_NAME}"
echo "TXT_VALUE:${TXT_VALUE}"

# Call the GoDaddy API to add the DNS TXT record
curl -X PUT -H "Authorization: sso-key ${GD_Key}:${GD_Secret}" \
  -H "Content-Type: application/json" \
  -d "[{\"name\": \"${TXT_NAME}\", \"type\": \"TXT\", \"data\": \"${TXT_VALUE}\", \"ttl\": 600}] " \
  "https://api.godaddy.com/v1/domains/${DOMAIN}/records/TXT/${TXT_NAME}"

# dig
for i in {1..6}; do
    dig_value=$(dig +short ${TXT_NAME}.${DOMAIN} txt);
    echo "dig_value:${dig_value}";
    if [ ${dig_value} == ${TXT_VALUE} ]; then
        break;
    fi
    sleep 10
done

```

## executes certbot command
```shell
certbot certonly --manual --manual-auth-hook /path/godaddy-auth-hook-script.sh --preferred-challenges dns -d {DOMAIN}
```



