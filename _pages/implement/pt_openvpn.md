---
layout: single
author_profile: false

title: "Obfuscating OpenVPN"
permalink: /implement/openvpn/

excerpt: ""
header:
  title: "Obfuscating OpenVPN"
  overlay_image: /assets/images/unsplash_vomyzrxpr_4-geoffrey-arduini.jpg
  caption: "Photo credit: [**Unsplash / Geoffrey Arduini**](https://unsplash.com/@geoffreyarduini)"
#  cta_label: "More Info"
#  cta_url: "https://unsplash.com"

sidebar:
    nav: "sidenav"
---

{% include toc icon="file-text" %}

This guide steps you through setting up your own obfuscated OpenVPN system.

**If you'd prefer to get started using a pre-configured script, you can use this [OpenVPN and dispatcher Bash script](https://github.com/OpenInternet/openvpn-shapeshifter)**

# Introduction

VPNs are user friendly and easy to manage service being used by users in countries with censorship which made it a target to government firewalls.  

<img src="/assets/images/OpenVPNconnection.png" width="50%" alt="VPN circumventing a firewall" />

*VPN circumventing a firewall*

However, VPNs are increasingly targeted for blocking themselves. Deep Packet Inspection DPI is able to block any targeted application or protocol by filtering the traffic and preventing it from passing firewalls to the outside world Internet. As DPI firewalls act based on the packet type, not the port number, simple "tricks" like changing the port will not help

 Pluggable transports make it possible to bypass such filtering without modifying the VPN itself but proxying the traffic into obfuscated tunnels which are significantly more difficult to identify and/or are costly to block to enable the traffic to pass through. Read more about [different types of obfuscation](/transports) or the [history of filtering](/how/#dpi-blocking/). 

<img src="/assets/images/DPIOpenVPN.png" alt="Obfusctated VPN circumventing a DPI firewall" />

*Obfusctated VPN circumventing a DPI firewall*

In this process, we will be using [shapeshifter-dispatcher](https://github.com/OperatorFoundation/shapeshifter-dispatcher/blob/master/README.md) which is a command line proxy tool which will act as a PT to help OpenVPN proxy the traffic through the firewall:

“The Shapeshifter project provides network protocol shapeshifting technology (also sometimes referred to as obfuscation). The purpose of this technology is to change the characteristics of network traffic so that it is not identified and subsequently blocked by network filtering devices.”

This tutorial uses Dispatcher to enable OpenVPN traffic to pass DPI firewalls, but it is important to note that this concept and process can apply to other services, including but not limited to VOIP and SSH or any other UDP or TCP connection.

# Preparation

First, we will be installing the following packages that you will use during the installation

* Openssl, in case it was not installed, this will allow you to generate ssl certificate for the server and the clients
* ca-certificates, the common CA certificates
* git, distributed version control system, we will need it to setup shapeshifter-dispatcher
* golang, Go programing language, which is needed to get shapeshifter-dispatcher to work
* curl,  to get files from web or ftp server.
* screen, this application will help you to run processes in the background

~~~~
apt-get update
apt-get install openssl ca-certificates git golang curl screen -y
~~~~

# OpenVPN

In this step, we will install and configure OpenVPN Server on Ubuntu 16.04.1 LTS and test it in non-DPI environment to be sure that it’s working. Please note that the procedure will probably work on any Debian / Ubuntu distro. You must run the installation and configure the different applications as root or sudoers account.

**If you already have OpenVPN installed and running, skip forward to [installing Dispatcher](#server-obfuscation-configuration)**

~~~~
apt-get install openvpn 
~~~~

## Install and configure Certificates

In this step, you will create the PKI, setup the CA, DH parameters, the server and client certificates.

* Create CA directory:

~~~~
make-cadir ~/openvpn-ca
cd ~/openvpn-ca
~~~~

* You need to change the vars of the configuration you will generate, run the
    following commands and change the values:

~~~~
nano vars
~~~~

* change the following values to what you want it to be:

~~~~
export KEY_COUNTRY="US"
export KEY_PROVINCE="CA"
export KEY_CITY="SanFrancisco"
export KEY_ORG="Fort-Funston"
export KEY_EMAIL="me@myhost.mydomain"
export KEY_OU="MyOrganizationalUnit"
export KEY_NAME="server"
~~~~

* then run the following command to use the vars:

~~~~
source vars
~~~~

* Run the following command to remove all the old keys

~~~~
./clean-all
~~~~

* Now generate your CA root certificate

~~~~
./build-ca
~~~~

* Now generate your server key file

~~~~
./build-key-server server
~~~~

After you finish the process, it will ask you to verify the expiration day of the certificate and the permission to issue the certificate, answer (Y) to both requests.

* Now generate Diffie-Hellman keys


~~~~
./build-dh
~~~~

* Now generate your TLS integrity verification capabilities, which will
    improve the security of the server and prevent any denial-of-service
    attempts

~~~~
openvpn --genkey --secret keys/ta.key
~~~~

* Move the files we need to OpenVPN main folder:

~~~~
cp ca.crt ca.key server.crt server.key ta.key dh2048.pem /etc/openvpn/
~~~~

* Create the server.conf file:

~~~~
nano /etc/openvpn/server.conf
~~~~

* Add the following configurations to it

~~~~
mode server
tls-server
port 1194 #Change the port of OpenVPN to the one you want
proto tcp
dev tun
sndbuf 0
rcvbuf 0
ca ca.crt
cert server.crt
key server.key
dh dh2048.pem
tls-auth ta.key 0
topology subnet
server 10.8.0.0 255.255.255.0
ifconfig-pool-persist ipp.txt
push "redirect-gateway def1 bypass-dhcp"
~~~~

* You can change the DNS you will be using to anyone you want

~~~~
push "dhcp-option DNS 208.67.222.222"
push "dhcp-option DNS 208.67.220.220"
keepalive 10 120
cipher AES-256-CBC
comp-lzo
persist-key
persist-tun
status openvpn-status.log
verb 3
~~~~

### Configure the network

In this step, you will configure your network to allow OpenVPN traffic.  You will need to change the following values to the correct numbers:

* YOURSERVERIPADDRESS: The Public IP address of your server
* OPENVPNPORT: The port you will use for the OpenVPN Server
* OBFSPORT: The port you will use for shapeshifter-dispatcher

Allow IP forward, most of the Linux distributions will have IP Forwarding disabled for security reasons, but we will need to allow IP forward in order to allow the OpenVPN to function. By running the following command, we will check if IP Forwarding is enabled:

~~~~
sysctl net.ipv4.ip_forward
~~~~

if the value was 0:

~~~~
net.ipv4.ip_forward = 0
~~~~

that means IP forward is disabled, to enable it run the following command:

~~~~
echo 1 > /proc/sys/net/ipv4/ip_forward
~~~~

Set NAT for the VPN subnet:

~~~~
iptables -t nat -A POSTROUTING -s 10.8.0.0/24 -j SNAT --to YOURSERVERIPADDRESS
    iptables -I INPUT -p tcp --dport OPENVPNPORT -j ACCEPT
    iptables -I FORWARD -s 10.8.0.0/24 -j ACCEPT
    iptables -I FORWARD -m state --state RELATED,ESTABLISHED -j ACCEPT
~~~~

### Start the OpenVPN service

~~~~
/etc/init.d/openvpn restart
systemctl start openvpn@server
~~~~

* Now check if OpenVPN is running by checking if the tun0 interface is available

~~~~
ip addr show tun0
~~~~

* if you see a configured interface (Assigned IP address to tun) run the
    following command to enable OpenVPN:

~~~~
systemctl enable openvpn@server
~~~~

* Add a client:
  
In this step, you will create the key, certificate and configuration files
    for your first OpenVPN user. The configuration file will have the client
    certificate embedded.

~~~~
cd ~/openvpn-ca/
source ./vars
~~~~

*Create a key file named CLIENT

~~~~
./pkitool CLIENT
~~~~

* This process will generate CLIENT.key and CLIENT.crt, move them to OpenVPN folder

~~~~
cd keys/
cp CLIENT.key CLIENT.crt /etc/openvpn
~~~~

* Create the client configuration file:

~~~~
nano CLIENT.ovpn
~~~~


* Then add the following configurations to the file

~~~~
client
dev tun
proto tcp
sndbuf 0
rcvbuf 0
remote YOURSERVERIPADDRESS #Change this one to your public IP address
port 1194 #Change this one to the port you’re using for OpenVPN
resolv-retry infinite
nobind
persist-key
persist-tun
remote-cert-tls server
cipher AES-256-CBC
comp-lzo
setenv opt block-outside-dns
key-direction 1
verb 3
ls-auth ta.key 1
ca ca.crt
cert CLIENT.crt
key CLIENT.key
~~~~

Save the file, name it CLIENT.ovpn and put it it along with the following files ,CLIENT.crt, CLIENT.key, ta.key and ca.crt and test the OpenVPN connection.

If it worked -- continue to the installation of shapeshifter-dispatcher and enable obfuscation for OpenVPN.

# Server Obfuscation Configuration
   
In this step, we will install and use Shapeshifter-dispatcher Server and client on Ubuntu 16.04.1 LTS. Please note that the procedure will probably work on any Debian &amp; Ubuntu distro. You must run the the process as root or sudoers account.

**The installation process of Shapeshifter-dispatcher is the same on the server and client, so apply these steps on both.**

### Install and configure shapeshifter-dispatcher

~~~~
mkdir ~/go
export GOPATH=~/go
go get -u github.com/OperatorFoundation/shapeshifter-dispatcher/shapeshifter-dispatcher
~~~~

### Shapeshifter-dispatcher server and client setup:

Run shapeshifter-dispatcher on the server by running the following       command for Obfs2:

~~~
screen ~/go/bin/shapeshifter-dispatcher -server -transparent -ptversion 2
   -transports obfs2 -state state -bindaddr obfs2-YOURSERVERIPADDRESS:OBFSPORT
   -orport 127.0.0.1:OPENVPNPORT
~~~
You will get this screen. Don’t forget to change the IP address and ports within the command.

<<<<<<< HEAD
<img  src="https://www.pluggabletransports.info/assets/images/Obfsservera.png" alt="Server Setup results screenshot" />
=======
<img  src="http://pluggabletransports.info/assets/images/Obfsservera.png" alt="Server Setup results screenshot" />
>>>>>>> upstream/gh-pages

And the following command to use Obfs4:
~~~
bin/shapeshifter-dispatcher -transparent -server -state state -orport 127.0.0.1:3333 -transports obfs4 -bindaddr obfs4-127.0.0.1:2222 -logLevel DEBUG -enableLogging -extorport 127.0.0.1:3334
~~~
When the server is run for the first time, it will generate a new public key and it will write it to a file in the state directory called obfs4_bridgeline.txt. This information is needed by the dispatcher client. Look in the file and retrieve the public key from the bridge line. It will look similar to this:
~~~
Bridge obfs4 <IP ADDRESS>:<PORT> <FINGERPRINT> cert=OfQAPDamjsRO90fDGlnZR5RNG659FZqUKUwxUHcaK7jIbERvNU8+EVF6rmdlvS69jVYrKw iat-mode=0
~~~

Set NAT for shapeshifter-dispatcher on the server:   

~~~~
iptables -I INPUT -p tcp --dport OBFSPORT -j ACCEPT
~~~~   

Run shapeshifter-dispatcher on the client side for Obfs2:

~~~~
screen ~/go/bin/shapeshifter-dispatcher -client -transparent -ptversion 2
    -transports obfs2 -state state -target YOURSERVERIPADDRESS:OBFSPORT
~~~~
You will get this screen:

<img src="/assets/images/obfsclient.png" alt="screenshot of launching ptclientproxy, success at listening stage" />

Or the following command to use Obfs4, don't forget to change the value of 'cert' with the one you generated on the server:
~~~~
bin/shapeshifter-dispatcher -transparent -client -state state -target 127.0.0.1:2222  -transports obfs4 -bindaddr obfs4-127.0.0.1:443 -options '{"cert": "OfQAPDamjsRO90fDGlnZR5RNG659FZqUKUwxUHcaK7jIbERvNU8+EVF6rmdlvS69jVYrKw", "iatMode": "0"}' -logLevel DEBUG -enableLogging
~~~~

For more information on how to use Obfs4, check the [offical guide of Shapeshifter-Dispatcher](https://github.com/OperatorFoundation/shapeshifter-dispatcher/blob/master/README.md) 

# Client Obfuscation Configuration

After running both the server and the client side, shapeshifter-dispatcher will connect and handshake which will then establish the connection between both sides.

Now you will be able to reconfigure the client’s OpenVPN configuration file to send OpenVPN requests and packets to the client shapeshifter-dispatcher which will then establish the obfuscated connection with shapeshifter-dispatcher server which will receive the obfuscated packets and forward the OpenVPN client connection to the OpenVPN Server.

Shapeshifter-dispatcher uses the port 1234 on the client side by default which means that this is the port which OpenVPN will talk to on the client machine.

You need to open the file *CLIENT.ovpn* and change the following values to point the VPN client at the local Shapeshifter-dispatcher for obfuscation and routing:

~~~~
remote 127.0.0.1 
port 1234
~~~~
Save the file and establish the connection, you’re now off the radar!

Note: We used obfs2 in this example to explain how Pluggable transport can be used with OpenVPN only, Obfs2 is censored in many places and you might use other transports like [Obfs4 – Learn how to use it!]( https://www.pluggabletransports.info/blog/ShapeshifterDispatcherObfs4/)
