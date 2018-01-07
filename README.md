simple-nginx-letsencrypt-docker
==============================================
This is a simple NGINX proxy Docker image that uses Lets Encrypt's certbot to set up HTTPS.

There are some really cool solutions for this such as [jwilder's nginx-proxy](https://github.com/jwilder/nginx-proxy) and [staticfloat's docker-nginx-certbot](https://github.com/staticfloat/docker-nginx-certbot). This one exists for my own benefit, because I wanted a better understanding of what was going on than I got with more automatic solutions. The entrypoint script in this image is based off the one in staticfloat's repo, just less smart.

Functionality
=============
On startup the entrypoint.sh script will check if a cert already exists for your domain. If it doesn't it will call certbot to get one (and sign you up for emails, sorry). By default it will get a test cert to avoid running into usage limits from Let's Encrypt during the flailing phase.

Usage
-----
1. Set up your site in conf.d as is shown in static-test.conf, or mount a volume to /etc/nginx/conf.d/ to replace that with your own.
2. Mount a volue at /etc/letsencrypt if you want to make the certs you get from Let's Encrypt persistent.
3. Provide the following environment variables to your container:
    * CERTBOT_EMAIL: <the email to provide to Let's Encrypt>
    * CERTBOT_DOMAIN: <the domain to get certs for>
    * PROD: <true to use production Let's Encrypt, any other value or blank to use their test server>

certbot will alter your conf.d/* configuration and create some files in /etc/letsencrypt, so when you switch from testing to prod or want to start fresh you may need to clean up those changes.
