# HAProxy installation
```bash
sudo apt-get install haproxy
```

# Configuraton
```bash
sudo vim /etc/haproxy/haproxy.cfg
...

# defines fontend
frontend http_frontend
	bind *:80
    bind *:443 ssl crt /etc/haproxy/ssl/mydomain.pem
	reqadd X-Forwarded-Proto:\ http

	# define host
	acl host_shellinabox hdr(host) -i shellinabox.chomookun.net
	acl host_code-server hdr(host) -i code-server.chomookun.net

	# figure out which one to use
	use_backend shellinabox if host_shellinabox
	use_backend code-server if host_code-server

	# default
	default-backend shellinabox

# service shellinabox
backend shellinabox
	balance roundrobin
	server server1 127.0.0.1:4200

# service code-server
backend code-server
	balance roundrobin
	server server1 127.0.0.1:8080

...

```

# Generates self-signed certificates
```bash
# creates directory
sudo mkdir /etc/haproxy/ssl && cd /etc/haproxy/ssl

# Generate a unique private key (KEY)
sudo openssl genrsa -out mydomain.key

# Generating a Certificate Signing Request (CSR)
sudo openssl req -new -key mydomain.key -out mydomain.csr

# Creating a Self-Signed Certificate (CRT)
sudo openssl x509 -req -days 3650 -in mydomain.csr -signkey mydomain.key -out mydomain.crt

# Append KEY and CRT to mydomain.pem
sudo bash -c 'cat mydomain.key mydomain.crt >> mydomain.pem'

```

