---
layout: single
author_profile: false
permalink: /implement/shapeshifter/

title: "Installing shapeshifter-dispatcher"

header:
  overlay_image: /assets/images/unsplash_0wdpet-ufqs-todd-diemer.jpg
  caption: "Photo credit: [**CC0 Unsplash / Todd Diemer**](https://unsplash.com/@todd_diemer)"
#  cta_label: "More Info"
#  cta_url: "https://unsplash.com"

sidebar:
    nav: "impnav"
---

{% include toc icon="file-text" %}

This guide assumes you already have a server installed with Go. If you need help, please follow our [Basic Server Setup](/implement/basicserver#installing-go) guide. The instructions below are adapted from [https://github.com/OperatorFoundation/shapeshifter-dispatcher](https://github.com/OperatorFoundation/shapeshifter-dispatcher). They will take you through downloading shapeshifter and proving that it works at its most basic level. For using shapeshifter-dispatcher with OpenVPN, see [our guide here](/implement/openvpn).

# Downloading and building shapeshifter-dispatcher #

This will install shapeshifter-dispatcher into its own directory one level down from your home directory. You should do this on __both__ your server and client machine.

~~~~~
cd ~
git clone https://github.com/OperatorFoundation/shapeshifter-dispatcher
~~~~~

Now, navigate to that directory, and build shapeshifter-dispatcher with all of its dependencies.

~~~~~
cd ~/shapeshifter-dispatcher
go build
~~~~~

To prove that the program itself is working, run this command:
~~~~~
./shapeshifter-dispatcher
~~~~~

That will return basic usage instructions and an error to let you know that you need parameters to successfully run the program.

# Using Netcat #
We're going to use Netcat, a network tool to communicate between machines. If you already know that Netcat is working, you can skip to the section [Configuring Netcat to use shapeshifter](#configuring-netcat-to-use-shapeshifter). At its simplest, information typed into the client can be displayed on the server, and vice versa.

Let's try it on the server and client you're using, over port 3344. If you followed our [Basic Server Setup](/implement/basicserver#installing-go) guide, you will need to allow traffic to and from port 3344 on your server. To do this, use the following command:

~~~~~
sudo ufw allow proto tcp from any to any port 3344
~~~~~

It should return a statement to tell you that the rule has been updated


On the server, run the command:

~~~~~
nc -l 3344
~~~~~

This will start a server session, listening on port 3344. We're going to use the example address 203.0.113.101 for the server. You will replace that with your server's IP address. On your client, use this command to connect to port 3344 on your server:

~~~~~
nc 203.0.113.101 3344
~~~~~

Now type in anything you like, and it will also display on your server. Type something on your server, and it will display on the client. This shows that our server and client are able to talk to each other. Now we're going to get them to talk over shapeshifter.

# Configuring Netcat to use shapeshifter #
Now that you know you can use Netcat, you can configure shapeshifter to use it. In this case, traffic is received on your server by shapeshifter, and passed to the port that Netcat is listening to. On the client, Netcat sends its traffic to shapeshifter to obfuscate and send to the server. This means the path for data sent to the server and back is:

Client: Netcat -> Client: shapeshifter ---> Server: shapeshifter -> Server: Netcat -> Server: shapeshifter ---> Client: shapeshifter -> Client: Netcat

We're going to use obfs2 for this first example, which is the simplest configuration for shapeshifter. We'll add other transports in the near future!

To prepare, you need to know what ports to use, and you may need to enable them on your server as in the [previous section](#using-netcat). First, let's configure the server to use Netcat on port 3344, and shapeshifter on port 2233. You'll need to make sure port 2233 is open. If you are using ufw, you would do this with the following command:

~~~~~
sudo ufw allow proto tcp from any to any port 2233
~~~~~


In one window, run the shapeshifter command, substituting the IP address 203.0.113.101 with your server's IP address:

~~~~~
cd ~/shapeshifter-dispatcher
./shapeshifter-dispatcher -server -transparent -ptversion 2 -transports obfs2 -state state -bindaddr obfs2-203.0.113.101:2233 -orport 127.0.0.1:3344 -enableLogging -logLevel DEBUG
~~~~~

Note that this binds obfs2 to port 2233, while it will send traffic to port 3344, where Netcat will be listening.

In a separate server session, run this command to start Netcat listening on port 3344:

~~~~~
nc -l 3344
~~~~~

Now switch to your client. On this machine, we're going to configure shapeshifter to talk to the server's shapeshifter, and for Netcat to connect via shapeshifter.

To do this, you will be setting a proxy port on your client to connect Netcat to. This is the port that shapeshifter will be listening on, and in our example we're going to use port 4455. Don't forget to change the 203.0.113.101 address to the IP address of your server.

~~~~~
cd ~/shapeshifter-dispatcher
./shapeshifter-dispatcher -transparent -client -state state -target 203.0.113.101:2233 -transports obfs2 -proxylistenaddr 127.0.0.1:4455 -logLevel DEBUG -enableLogging
~~~~~

Now you're going to connect Netcat to that proxy, and it will talk to your server using shapeshifter.

~~~~~
nc 127.0.0.1 4455
~~~~~

Anything you type into Netcat will be seen on your server, and vice versa. Congratulations! You've go shapeshifter up and running. What's next? Perhaps you want to take it to the next level and try our [guide to OpenVPN](/implement/openvpn).