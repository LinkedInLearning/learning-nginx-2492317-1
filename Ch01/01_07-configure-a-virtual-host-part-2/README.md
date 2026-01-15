# 01_07 Configure a virtual host Part 2

[binaryville.conf](./binaryville.conf)

```NGINX
server {
    listen 80;
    root /var/www/binaryville;
}
```

After the virtual host configuration is finished, the `binaryville.conf` should be as follows:

[binaryville.finished.conf](./binaryville.finished.conf)

```NGINX
server {
    listen 80 default_server;
    root /var/www/binaryville;

    server_name binaryville.local www.binaryville.local;
    index index.html index.htm index.php;
}
```

<!-- FooterStart -->
---
[← 01_06 Configure a virtual host Part 1](../01_06-configure-a-virtual-host-part-1/README.md) | [01_08 Add files to the root directory →](../01_08-add-files-to-the-root-directory/README.md)
<!-- FooterEnd -->
