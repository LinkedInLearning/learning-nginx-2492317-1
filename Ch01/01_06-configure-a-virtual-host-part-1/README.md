# 01_06 Configure a virtual host Part 1

NGINX configurations are based on "server blocks" that use the server_name and listen directives to bind to TCP sockets.

[Documentation for `server` from `ngx_http_core_module`](http://nginx.org/en/docs/http/ngx_http_core_module.html#server)

## Basic server block configuration

[binaryville.conf](./binaryville.conf)

```NGINX
server {
    listen 80;
    root /var/www/binaryville;
}
```

<!-- FooterStart -->
---
[← 01_05 Inside nginx.conf](../01_05-inside-nginx-conf/README.md) | [01_07 Configure a virtual host Part 2 →](../01_07-configure-a-virtual-host-part-2/README.md)
<!-- FooterEnd -->
