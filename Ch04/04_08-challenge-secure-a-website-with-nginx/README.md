# 04_08 Challenge: Secure a website with NGINX

Secure a website using NGINX.

## Requirements

1. Install NGINX and remove the default configuration
1. Add a new configuration for the preview site
1. Configure basic authentication for the entire site

    [preview.conf](./preview.conf)

    ```NGINX
    server {
        listen 80 default_server;
        # ADD BASIC AUTH CONFIGURATION HERE
    }
    ```
1. Allow  access with username “CEO” with password “qwerty”
1. Validate the configuration with a browser

<!-- FooterStart -->
---
[← 04_07 Configure NGINX to use an SSL certificate](../04_07-configure-nginx-to-use-an-ssl-certificate/README.md) | [04_09 Solution: Secure a website with NGINX →](../04_09-solution-secure-a-website-with-nginx/README.md)
<!-- FooterEnd -->
