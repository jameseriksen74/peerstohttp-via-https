This solves the problem related to streaming to a ssl secured website (https)

1) install nginx

apt update
apt install nginx

2) remove default config

unlink /etc/nginx/sites-enabled/default

3) go to sites-available and create the config file

cd /etc/nginx/sites-available

nano reverse-proxy.conf

upstream peerstohttp {
        server 127.0.0.1:8484;
}


#you can run on the port of your choice

server {
  listen 80;
  server_name listing-test.cnyawscloud3.com;

    listen 443 ssl; # managed by Certbot
    ssl_certificate /path/to/fullchain.pem; # Recommend - managed by Certbot
    ssl_certificate_key /path/to/privkey.pem; # Recommend - managed by Certbot
    include /etc/letsencrypt/options-ssl-nginx.conf; # Recommend - managed by Certbot
    ssl_dhparam /etc/letsencrypt/ssl-dhparams.pem; # Recommend - managed by Certbot
  location / {
        proxy_pass http://peerstohttp;
    }
}


4) add TLS / SSL to your reverse proxy

apt-get update
apt-get install software-properties-common
add-apt-repository ppa:certbot/certbot
apt-get update
apt-get install python-certbot-nginx


5) run cerbot

certbot --nginx

6) add your details

Saving debug log to /var/log/letsencrypt/letsencrypt.log
Plugins selected: Authenticator nginx, Installer nginx

Which names would you like to activate HTTPS for?
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
1: your.domain.com
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
Select the appropriate numbers separated by commas and/or spaces, or leave input
blank to select all options shown (Enter 'c' to cancel): 1
Obtaining a new certificate
Performing the following challenges:
http-01 challenge for your.domain.com
Waiting for verification...
Cleaning up challenges
Deploying Certificate to VirtualHost /etc/nginx/sites-enabled/reverse-proxy.conf

Please choose whether or not to redirect HTTP traffic to HTTPS, removing HTTP access.
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
1: No redirect - Make no further changes to the webserver configuration.
2: Redirect - Make all requests redirect to secure HTTPS access. Choose this for
new sites, or if you're confident your site works on HTTPS. You can undo this
change by editing your web server's configuration.
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
Select the appropriate number [1-2] then [enter] (press 'c' to cancel): 2
Redirecting all traffic on port 80 to ssl in /etc/nginx/sites-enabled/reverse-proxy.conf

- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
Congratulations! You have successfully enabled https://your.domain.com

You should test your configuration at:
https://www.ssllabs.com/ssltest/analyze.html?d=your.domain.com
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -

