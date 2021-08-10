<h1> In this project, we will enhance our Tooling Website solution by configuring a Load Balancer nginx with SSL/TLS</h1>

<p>This project consists of two parts:</p>

    1. Configure Nginx as a Load Balancer

    2. Register a new domain name and configure secured connection using SSL/TLS certificates

Your target architecture will look like this:

![nginx_lb](https://user-images.githubusercontent.com/10243139/128805566-0f54801f-e2b7-47b4-983a-2d8d2b6cee44.png)

<h2>Part 1 – Configure Nginx As A Load Balancer</h2>

<p>Create an EC2 VM based on Ubuntu Server 20.04 LTS and name it Nginx LB (do not forget to open TCP port 80 for HTTP connections, also open TCP port 443 – this port is used for secured HTTPS connections)</p>

<p>Update /etc/hosts file for local DNS with Web Servers’ names (e.g. Web1 and Web2) and their local IP addresses</p>

<p>Install and configure Nginx as a load balancer to point traffic to the resolvable DNS names of the webservers:</p>

    sudo apt update
    sudo apt install nginx

<p>Configure Nginx LB using Web Servers’ names defined in /etc/hosts</p>

    sudo nano /etc/nginx/nginx.conf

<p>insert following configuration into http section</p>

     upstream myproject {
        server Web1 weight=5;
        server Web2 weight=5;
      }

    server {
        listen 80;
        server_name www.domain.com;
        location / {
          proxy_pass http://myproject;
        }
      }

<p>#comment out this line</p>

    #include /etc/nginx/sites-enabled/*;

<p>Restart Nginx and make sure the service is up and running</p>

    sudo systemctl restart nginx

<p>Confirm the NGINX Server by entering the public ip address to see it works</p>

![1 e](https://user-images.githubusercontent.com/10243139/128806099-ec73e673-20a9-4abd-9bd8-06cfac7e155c.jpg)

