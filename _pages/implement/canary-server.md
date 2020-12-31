---
layout: single
author_profile: false
permalink: /implement/canary-server/

title: "Configuring a server for Canary"

header:
  overlay_image: /assets/images/unsplash_0wdpet-ufqs-todd-diemer.jpg
  caption: "Photo credit: [**CC0 Unsplash / Todd Diemer**](https://unsplash.com/@todd_diemer)"
#  cta_label: "More Info"
#  cta_url: "https://unsplash.com"

sidebar:
    nav: "impnav"
---

{% include toc icon="file-text" %}

This guide assumes you already have a server installed with Go and shapeshifter-dispatcher. If you need help, please follow our [Basic Server Setup](/implement/basicserver#installing-go) guide and the guide to configuring [shapeshifter-dispatcher](/implement/shapeshifter). The instructions below are adapted from [https://github.com/OperatorFoundation/Canary](https://github.com/OperatorFoundation/Canary). This guide is for configuring a server to run multiple transports that can be tested by the Canary client.

You will need NGINX to act as a web server, and Supervisor to control multiple instances of shapeshifter-dispatcher.

# Downloading and installing NGINX
NGINX is a web server and reverse proxy tool. We will be using it to serve a file that is needed by the Canary client to check that a web page is available. To download and install NGINX, run this command:

~~~~~
sudo apt install nginx
~~~~~

By default, the server will run on port 80. If you want to change that, for example to port 88 for Canary tests, run this command:

~~~~~
sudo nano /etc/nginx/sites-available/default.conf
~~~~~

Change this lines to make NGINX run on port 88, then press Ctrl-X followed by Y to save the file.

~~~~~
server {
        listen 88 default_server;
~~~~~

Start NGINX with systemctl start, then check that it is running with systemctl status.

~~~~~
systemctl start nginx
systemctl status nginx
~~~~~

If your server has a single CPU, you might see a message like this:

~~~~~
nginx.service: Failed to parse PID
~~~~~

The following commands are a workaround to fix this known issue, found at (https://bugs.launchpad.net/ubuntu/+source/nginx/+bug/1581864_[https://bugs.launchpad.net/ubuntu/+source/nginx/+bug/1581864])

~~~~~
  sudo mkdir /etc/systemd/system/nginx.service.d
  printf "[Service]\nExecStartPost=/bin/sleep 0.1\n" | \
    sudo tee /etc/systemd/system/nginx.service.d/override.conf
  sudo systemctl daemon-reload
  sudo systemctl restart nginx
~~~~~

Canary tests for a single word at the top of the web page. You can change the default index page by editing this file:

~~~~~
sudo nano /var/www/html/index.hml
~~~~~

The file should simply read:

~~~~~
Yeah!
~~~~~

Hit Ctrl-X and press Y to save the file.

# Downloading and configuring Supervisor #

Supervisor is a \"process control system\", allowing you to launch, monitor and control processes on your server. To install it, use the command:

~~~~~
sudo apt install supervisor
~~~~~

You will now need to create the environment in which to run multiple copies of shapeshifter-dispatcher, each with a different transport configuration. In the sample config files below, we're pointing to specific directories that need to be created:

~~~~~
mkdir ~/transportConfigs
mkdir ~/gofiles
~~~~~

We're also using easy-to-spot port names. You should change these for your configuration.

You will now create .conf files for Supervisor to load. They are all kept in \etc\supervisor\conf.d, and have the names obfs2.conf, obfs4.conf, shadow.conf and replicant.conf. Let's create obfs2.conf using Nano:

~~~~~
sudo nano /etc/supervisor/conf.d/obfs2.conf
~~~~~

The contents of the file should look similar to this:

~~~~~
[program:obfs2]
command=/home/username/shapeshifter-dispatcher/shapeshifter-dispatcher -server -transparent -ptversion 2 -transports obfs2 -state state -bindaddr obfs2-127.0.0.1:2233 -orport 127.0.0.1:88
directory=/home/username/gofiles/
autostart=true
autorestart=true
stderr_logfile=/var/log/obfs2.err.log
stdout_logfile=/var/log/obfs2.out.log
~~~~~ 

This will bind obfs2 to port 2233, and direct traffic to port 88, the NGINX server.

Next, create obfs4.conf:

~~~~~
[program:obfs4]
command=/home/ubuntu/shapeshifter-dispatcher/shapeshifter-dispatcher -server -transparent -ptversion 2 -transports obfs4 -state state -bindaddr obfs4-0.0.0.0:1234 -orport 127.0.0.1:80 -extorport 127.0.0.1:3334
directory=/home/ubuntu/gofiles
autostart=true
autorestart=true
stderr_logfile=/var/log/shapeshifter.err.log
stdout_logfile=/var/log/shapeshifter.out.log
~~~~~

When it is first run, this will create obfs4_bridgeline.txt, from which you can get the key for the client. See our [guide to obfs4](https://www.pluggabletransports.info/implement/shapeshifter/#obfs4) for more details.

Shadowsocks needs a config file to run. To do this, you can create a file at \~/transportConfigs/shadowServer.json, with contents similar to this (choosing your own password):

~~~~~
{"shadow": {"password": "1234", "cipherName": "CHACHA20-IETF-POLY1305"}}
~~~~~

Now create /etc/supervisor/conf.d/shadow.conf:

~~~~~
[program:shadow]
command=/home/ubuntu/shapeshifter-dispatcher/shapeshifter-dispatcher -server -transparent -ptversion 2 -transports shadow -state state -bindaddr shadow-0.0.0.0:2345 -orport 127.0.0.1:80 -extorport 127.0.0.1:3334 -logLevel DEBUG -enableLogging -optionsFile /home/ubuntu/transportConfigs/shadowServer.json
directory=/home/ubuntu/gofiles/
autostart=true
autorestart=true
stderr_logfile=/var/log/shadow.err.log
stdout_logfile=/var/log/shadow.out.log
~~~~~

The final file to create is Replicant. That also needs a config file to be created in \~/transportConfigs/replicant.json:

~~~~~
{"Replicant":{"config":"Your_replicant_key"}}
~~~~~

Now create /etc/supervisor/conf.d/replicant.conf:

~~~~~
[program:replicant]
command=/home/ubuntu/shapeshifter-dispatcher/shapeshifter-dispatcher -server -transparent -ptversion 2 -transports Replicant -state state -bindaddr Replicant-0.0.0.0:3456 -orport 127.0.0.1:80 -extorport 127.0.0.1:3334 -logLevel DEBUG -enableLogging -optionsFile "/home/ubuntu/transportConfigs/replicant.json"
directory=/home/ubuntu/gofiles/
autostart=true
autorestart=true
stderr_logfile=/var/log/Replicant.err.log
stdout_logfile=/var/log/Replicant.out.log
~~~~~


You're now ready to run Supervisor! To start it, and then check the status, use these commands:

~~~~~
sudo systemctl start supervisor
sudo supervisorctl status
~~~~~

This will return a list of processes being controlled by Supervisor. You should see something like:

~~~~~
obfs2                            RUNNING   pid 19035, uptime 3:40:18
obfs4                            RUNNING   pid 19039, uptime 3:40:18
replicant                        RUNNING   pid 19037, uptime 3:40:18
shadow                           RUNNING   pid 19036, uptime 3:40:18
~~~~~

Congratulations! Your server is now running the processes required for Canary testing.