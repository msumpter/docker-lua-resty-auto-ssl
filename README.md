# docker-lua-resty-auto-ssl
Docker file to launch OpenResty with automatic SSL generation provided by lua-resty-auto-ssl

All of the heavy lifting has been done here: https://github.com/GUI/lua-resty-auto-ssl

Getting started
------------
~~~
[mats@node0 projects]$ git clone https://github.com/msumpter/docker-lua-resty-auto-ssl.git
Cloning into 'docker-lua-resty-auto-ssl'...
remote: Counting objects: 10, done.
remote: Compressing objects: 100% (9/9), done.
remote: Total 10 (delta 1), reused 6 (delta 1), pack-reused 0
Unpacking objects: 100% (10/10), done.
[mats@node0 projects]$ cd docker-lua-resty-auto-ssl/
[mats@node0 docker-lua-resty-auto-ssl]$ ls
Dockerfile  LICENSE  nginx.conf  README.md
[mats@node0 docker-lua-resty-auto-ssl]$ docker build .
Sending build context to Docker daemon 65.54 kB
Step 1 : FROM openresty/openresty:latest-xenial
 ---> b66a65e18fc6
Step 2 : RUN /usr/local/openresty/luajit/bin/luarocks install lua-resty-auto-ssl
 ---> Using cache
 ---> f16591575aee
Step 3 : RUN openssl req -new -newkey rsa:2048 -days 3650 -nodes -x509 -subj '/CN=sni-support-required-for-valid-ssl' -keyout /etc/ssl/resty-auto-ssl-fallback.key -out /etc/ssl/resty-auto-ssl-fallback.crt
 ---> Running in 1dec579e51f5
Generating a 2048 bit RSA private key
..........................................................................................................................+++
..............+++
writing new private key to '/etc/ssl/resty-auto-ssl-fallback.key'
-----
 ---> 30eeea0304ac
Removing intermediate container 1dec579e51f5
Step 4 : ADD nginx.conf /usr/local/openresty/nginx/conf/nginx.conf
 ---> 2fd0b693f93c
Removing intermediate container d1f8adc5de16
Step 5 : ENTRYPOINT /usr/local/openresty/nginx/sbin/nginx -g daemon off;
 ---> Running in 9261b5df84e3
 ---> 61ccf7b04c8c
Removing intermediate container 9261b5df84e3
Successfully built 61ccf7b04c8c
[mats@node0 docker-lua-resty-auto-ssl]$ docker run -p 80:80 -p 443:443 61ccf7b04c8c    
~~~
