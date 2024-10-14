Default Admin User:
```
    Email:    admin@example.com
    Password: changeme
```
To make Cockpit proxy aware, you will need to modify the Cockpit config file located at /etc/cockpit/cockpit.conf. 
This file may not exist and if it doesn't you should create it.
```
    sudo nano /etc/cockpit/cockpit.conf
```
```
    [WebService]
    Origins = https://cockpit.domain.tld wss://cockpit.domain.tld https://192.168.1.222:9090
    ProtocolHeader = X-Forwarded-Proto
```
```
   sudo systemctl restart cockpit.service
```
Nginx as a reverse proxy for Cockpit, ensure that Nginx is configured to correctly forward TLS headers to Cockpit. 
In the Nginx configuration Advanced tab for Cockpit, add:

```
    proxy_set_header X-Forwarded-Proto $scheme;
    proxy_set_header X-Forwarded-For $remote_addr;
```

### NGINX Proxy Manager Compose (customized)

```
services:
  proxy:
    image: 'jc21/nginx-proxy-manager:latest'
    container_name: nginx-proxy-manager
    restart: unless-stopped
    network_mode: host
    volumes:
      - proxy-data:/data
      - proxy-letsencrypt:/etc/letsencrypt
    healthcheck:
      test: ["CMD", "/usr/bin/check-health"]
      interval: 10s
      timeout: 3s
volumes:
  proxy-data:
  proxy-letsencrypt:
```
