# WordPress

## Running Stack
* Direct Connection to the Internet since NGINX will install cURL
* Install [Docker Compose](https://docs.docker.com/compose/install/)
* Need to addto your **/etc/hosts** the record 127.0.0.1 **wordpress.example.org**

## MySQL

### Version
* MySQL 5.7.25

### Configuration

### Data
* DB saved to docker volume db-data
* Default port 3306
* User/Password Information on docker-compose.yaml

## NGINX

### Version
* NGINX 1.15.10

### Dockerfile
* Container installs curl (HealthCheck)

### Configuration

#### TLS
* Port 80 FWDing to port 443
* DH Enable (2048 bits)

```
openssl dhparam -out dhparam2048.pem 2048
```

* Create a Self Sign Cert since I do not have a DNS to put the wordpress.example.org

```
openssl genrsa -out rootCA.key 4096
openssl req -x509 -new -nodes -key rootCA.key -sha256 -days 1024 -out rootCA.crt
openssl genrsa -out wordpress.example.org.key 2048
openssl req -new -key wordpress.example.org.key -out wordpress.example.org.csr
openssl x509 -req -in wordpress.example.org.csr -CA rootCA.crt -CAkey rootCA.key -CAcreateserial -out wordpress.example.org.pem -days 500 -sha256
```

* HSTS Enable
* OCSP Stapling Enable

#### MicroCaching
* Added x-cache-status with bellow output for debugging purposes

```
HIT – The request is being served from the cache.
MISS – A cache key doesn’t exist for the current request and has been forwarded to PHP. The result of this request will be cached and subsequent requests will result in a HIT.
EXPIRED – A cache key exists, but the cache duration has elapsed. The request is forwarded to PHP which will regenerate the cache.
STALE – The content is stale because the origin server is not responding correctly, and proxy_cache_use_stale was configured.
UPDATING – The cache is currently being regenerated for the current request.
```

## PHP - WordPress

### Version
* WordPress 5.1.1

### Configuration
* Spin docker up and got through the admin config
* Create the first User, login and start using WordPress
