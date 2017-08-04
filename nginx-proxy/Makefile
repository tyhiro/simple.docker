WORKING_DIR=$(shell pwd)

net: @net
proxy: @proxy
proxy-helper: @proxy-helper

@net:
	docker network create reverse-proxy

@proxy:
	docker run -d -p 80:80 -p 443:443 \
        --name nginx-proxy \
        --net reverse-proxy \
        -v /var/www/docker/nginx-proxy/certs:/etc/nginx/certs:ro \
        -v /etc/nginx/vhost.d \
        -v /usr/share/nginx/html \
        -v /var/run/docker.sock:/tmp/docker.sock:ro \
        --label com.github.jrcs.letsencrypt_nginx_proxy_companion.nginx_proxy=true \
        jwilder/nginx-proxy

@proxy-helper:
	docker run -d \
        --name nginx-letsencrypt \
        --net reverse-proxy \
        --volumes-from nginx-proxy \
        -v /var/www/docker/nginx-proxy/certs:/etc/nginx/certs:rw \
        -v /var/run/docker.sock:/var/run/docker.sock:ro \
        jrcs/letsencrypt-nginx-proxy-companion




