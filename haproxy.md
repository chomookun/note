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
	acl host_ssh hdr(host) -i ssh.chomookun.net
	acl host_duice hdr(host) -i duice.chomookun.net

	# figure out which one to use
	use_backend ssh if host_ssh
	use_backend duice if host_duice

# service ssh
backend ssh
	balance roundrobin
	server server1 127.0.0.1:4200 ssl check verify none

# service duice
backend code-server
	balance roundrobin
	server server1 127.0.0.1:8080 ssl check verify none

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

