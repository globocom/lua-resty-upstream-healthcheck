# Add your local ip to /etc/hosts

```
<LOCAL_IP> test-tls.com
# do not use 127.0.0.1
```

# Run the selfsigned tls nginx upstream from this folder

```
docker run --rm -p 443:443 -v $(pwd)/nginx.conf:/usr/local/openresty/nginx/conf/nginx.conf -v $(pwd)/nginx-selfsigned.crt:/nginx-selfsigned.crt -v $(pwd)/nginx-selfsigned.key:/nginx-selfsigned.key openresty/openresty:xenial
```

# Run the downstream nginx (8080) from this folder

```
docker run --rm -p 8080:8080 -v $(pwd)/nginx-plain.conf:/usr/local/openresty/nginx/conf/nginx.conf -v $(pwd)/../lib/resty/upstream/:/lua/src/ openresty/openresty:xenial
```

# Do your tests

```
# check if it's working
curl localhost:8080 # it should respond "hello tls"

# check the status page
curl localhost:8080/status
```
