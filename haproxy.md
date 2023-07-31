# HAProxy installation
```bash
sudo apt-get install haproxy
```

# Configuraton
```bash
sudo vim /etc/haproxy/haproxy.cfg
...
lobal
    log /dev/log    local0
    log /dev/log    local1 notice
    chroot /var/lib/haproxy
    stats socket /run/haproxy/admin.sock mode 660 level admin expose-fd listeners
    stats timeout 30s
    user haproxy
    group haproxy
    daemon

    # Default SSL material locations
    ca-base /etc/ssl/certs
    crt-base /etc/ssl/private

    # See: https://ssl-config.mozilla.org/#server=haproxy&server-version=2.0.3&config=intermediate
        ssl-default-bind-ciphers ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-RSA-AES128-GCM-SHA256:ECDHE-ECDSA-AES256-GCM-SHA384:ECDHE-RSA-AES256-GCM-SHA384:ECDHE-ECDSA-CHACHA20-POLY1305:ECDHE-RSA-CHACHA20-POLY1305:DHE-RSA-AES128-GCM-SHA256:DHE-RSA-AES256-GCM-SHA384
        ssl-default-bind-ciphersuites TLS_AES_128_GCM_SHA256:TLS_AES_256_GCM_SHA384:TLS_CHACHA20_POLY1305_SHA256
        ssl-default-bind-options ssl-min-ver TLSv1.2 no-tls-tickets

defaults
    log global
    mode    http
    option  httplog
    option  dontlognull
        timeout connect 5000
        timeout client  50000
        timeout server  50000
    errorfile 400 /etc/haproxy/errors/400.http
    errorfile 403 /etc/haproxy/errors/403.http
    errorfile 408 /etc/haproxy/errors/408.http
    errorfile 500 /etc/haproxy/errors/500.http
    errorfile 502 /etc/haproxy/errors/502.http
    errorfile 503 /etc/haproxy/errors/503.http
    errorfile 504 /etc/haproxy/errors/504.http


# defines fontend
frontend http_frontend
    bind *:80
    bind *:443 ssl crt /home/chomookun/haproxy/ssl/oopscraft.org.pem
    mode http
    redirect scheme https code 301 if !{ ssl_fc }
    default_backend ingress

    # define host
    acl host_jenkins hdr(host) -i jenkins.oopscraft.org
    acl host_nexus hdr(host) -i nexus.oopscraft.org
    acl host_torrent hdr(host) -i torrent.oopscraft.org
    acl host_grafana hdr(host) -i grafana.oopscraft.org

    # figure out which one to use
    use_backend jenkins if host_jenkins
    use_backend nexus if host_nexus
    use_backend torrent if host_torrent
    use_backend grafana if host_grafana

frontend kube_api
    bind *:8443 ssl crt /home/chomookun/.minikube/profiles/minikube/apiserver.pem
    mode http
    redirect scheme https code 301 if !{ ssl_fc }
    default_backend kube_api

# backend argocd
backend ingress
    server server1 192.168.49.2:80
    #server server1 127.0.0.1:8080

backend jenkins
    server server1 127.0.0.1:9999

backend nexus
    server server1 127.0.0.1:9998

backend torrent
    server server1 127.0.0.2:9091

backend kube_api
    server server1 192.168.49.2:8443 check ssl verify none
...


