# 01_08 Add files to the root directory

With a server configuration in place, add some content to your website.

- Change to the root user and `cd` to the `/root` directory.

```bash
sudo su -
cd /root
```

- Clone the exercise files to the server.

```bash
git clone https://github.com/LinkedInLearning/learning-nginx-2492317-1.git
```

- Un-tar the archive into the NGINX root directory

```bash
tar xvf ~/learning-nginx-2492317-1/Binaryville_robot_website_LIL_107684.tgz --directory /var/www/binaryville/
```

- Confirm the files have been placed in the correct location

```bash
ls -ltr /var/www/binaryville/
```

- Open a browser and view the IP address or DNS name for your server

- Confirm the Binaryville website has been loaded

<!-- FooterStart -->
---
[← 01_07 Configure a virtual host Part 2](../01_07-configure-a-virtual-host-part-2/README.md) | [01_09 Challenge: Set up an NGINX server on Ubuntu Linux →](../01_09-challenge-set-up-an-NGINX-server-on-Ubuntu-Linux/README.md)
<!-- FooterEnd -->
