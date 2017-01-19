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
# How to obfuscate OpenVPN

This guide steps you through setting up your own obfuscated OpenVPN system.  If you'd prefer to get started using a pre-configured script, you can use this [OpenVPN and dispatcher Bash script](https://github.com/dlshad/openvpn-shapeshifter)

## Introduction

As it was explained in What is PT, Deep Packet Inspection DPI is able to block any targeted application or protocol by filtering the traffic and preventing it from passing firewalls to the outside world Internet. OpenVPN is one of the most targeted applications by DPI which prevents the most censored communities from accessing the Internet openly. Even changing the port will not help in this case as the firewall acts based on the packet type, not the port number.

In this process, we will be using [shapeshifter-dispatcher](https://github.com/OperatorFoundation/shapeshifter-dispatcher/blob/master/README.md) which is a command line proxy tool which will act as a PT to help OpenVPN proxy the traffic through the firewall:

“The Shapeshifter project provides network protocol shapeshifting technology (also sometimes referred to as obfuscation). The purpose of this technology is to change the characteristics of network traffic so that it is not identified and subsequently blocked by network filtering devices.”

<ol><li><strong id"opvpn_installation"> OpenVPN server and shapeshifter-dispatcher Installation: </strong>
        
    </li>
</ol>

If you already have OpenVPN installed and running, move forward to step II.
In this step, we will install and configure OpenVPN Server on Ubuntu
    16.04.1 LTS and test it in non-DPI environment to be sure that it’s
    working. Please note that the procedure will probably work on any Debian
    &amp; Ubuntu distro. You must run the installation and configure the
    different applications as root or sudoers account.

<ol><li>You need to downloads the most updated package lists:
    </li>
</ol>
<p dir="ltr">
	<code>
	apt-get update
	</code>
</p>
<ol start="2">
    <li dir="ltr">
        <p dir="ltr">
            In this step you will be installing the following packages that you
            will use during the installation and the operation stage:
        </p>
    </li>
    <ol>
        <li dir="ltr">
            <p dir="ltr">
                OpenVPN server packages
            </p>
        </li>
        <li dir="ltr">
            <p dir="ltr">
                Openssl, in case it was not installed, this will allow you to
                generate ssl certificate for the server and the clients
            </p>
        </li>
        <li dir="ltr">
            <p dir="ltr">
                ca-certificates, the common CA certificates
            </p>
        </li>
        <li dir="ltr">
            <p dir="ltr">
                git, distributed version control system, we will need it to
                setup shapeshifter-dispatcher
            </p>
        </li>
        <li dir="ltr">
            <p dir="ltr">
                golang, Go programing language, which is needed to get
                shapeshifter-dispatcher to work
            </p>
        </li>
        <li dir="ltr">
            <p dir="ltr">
                curl, it’s a tool that will help you to get files from web or
                ftp server.
            </p>
        </li>
        <li dir="ltr">
            <p dir="ltr">
                Screen, this application will help you to run processes in the
                background
            </p>
        </li>
    </ol>
</ol>
<p dir="ltr">
  <code>apt-get install openvpn openssl ca-certificates git golang curl screen -y</code>
</p>
<p>
    <strong>
        <br/>
    </strong>
</p>
<ol start="3">
    <li dir="ltr">
        <p dir="ltr">
            Install and configure shapeshifter-dispatcher, apply the following
            steps:
        </p>
    </li>
</ol>
<p dir="ltr">
    <code>mkdir ~/go</code>
</p>
<p dir="ltr">
    <code>export GOPATH=~/go</code>
</p>
<p dir="ltr">
  <code>go get -u github.com/OperatorFoundation/shapeshifter-dispatcher/shapeshifter-dispatcher</code>
</p>
<p>
    <strong>
        <br/>
    </strong>
</p>
<ol start="4">
    <li dir="ltr">
        <p dir="ltr">
            Create CA directory:
        </p>
    </li>
</ol>
<p dir="ltr">
   <code>make-cadir ~/openvpn-ca</code>
</p>
<p dir="ltr">
   <code>cd ~/openvpn-ca</code>
</p>
<ol start="5">
    <li dir="ltr">
        <p dir="ltr">
            Generate and configure the server certificates and configuration
            file:
        </p>
    </li>
</ol>
<p dir="ltr">
    In this step, you will create the PKI, setup the CA, DH parameters, the
    server and client certificates.
</p>
<p dir="ltr">
    You need to change the vars of the configuration you will generate, run the
    following commands and change the values:
</p>
<p dir="ltr">
    <code>cd ~/openvpn-ca</code>
</p>
<p dir="ltr">
   <code>nano vars</code>
</p>
<p dir="ltr">
    change the following values to what you want it to be:
</p>
<p dir="ltr">
    <code>export KEY_COUNTRY="US"</code>
</p>
<p dir="ltr">
    <code>export KEY_PROVINCE="CA"</code>
</p>
<p dir="ltr">
   <code> export KEY_CITY="SanFrancisco"</code>
</p>
<p dir="ltr">
    <code>export KEY_ORG="Fort-Funston"</code>
</p>
<p dir="ltr">
    <code>export KEY_EMAIL="me@myhost.mydomain"</code>
</p>
<p dir="ltr">
    <code>export KEY_OU="MyOrganizationalUnit"</code>
</p>
<p dir="ltr">
   <code>export KEY_NAME="server"</code>
</p>
<p>
    <strong>
        <br/>
    </strong>
</p>
<p dir="ltr">
    then run the following command to use the vars:
</p>
<p dir="ltr">
   <code> source vars</code>
</p>
<p dir="ltr">
    Run the following command to remove all the old keys
</p>
<p dir="ltr">
    <code>./clean-all</code>
</p>
<p dir="ltr">
    Now generate your CA root certificate
</p>
<p dir="ltr">
    <code>./build-ca</code>
</p>
<p dir="ltr">
    Now generate your server key file
</p>
<p dir="ltr">
    <code>./build-key-server server</code>
</p>
<p dir="ltr">
    After you finish the process, it will ask you to verify the expiration day
    of the certificate and the permission to issue the certificate, answer by
    (Y) to both requests.
</p>
<p dir="ltr">
    Now generate Diffie-Hellman keys
</p>
<p dir="ltr">
    <code>./build-dh</code>
</p>
<p dir="ltr">
    Now generate your TLS integrity verification capabilities, which will
    improve the security of the server and prevent any denial-of-service
    attempts
</p>
<p dir="ltr">
    <code>openvpn --genkey --secret keys/ta.key</code>
</p>
<p>
    <strong>
        <br/>
    </strong>
</p>
<ol start="6">
    <li dir="ltr">
        <p dir="ltr">
            Move the files we need to OpenVPN main folder:
        </p>
    </li>
</ol>
<p dir="ltr">
    <code>cp ca.crt ca.key server.crt server.key ta.key dh2048.pem /etc/openvpn/</code>
</p>
<p>
    <strong>
        <br/>
    </strong>
</p>
<ol start="7">
    <li dir="ltr">
        <p dir="ltr">
            Create the server.conf file:
        </p>
    </li>
</ol>
<p dir="ltr">
    <code>nano /etc/openvpn/server.conf</code>
</p>
<p>
    <strong>
        <br/>
    </strong>
</p>
<p dir="ltr">
    Then add the following configurations to it
</p>
<p dir="ltr">
    <code>mode server</code>
</p>
<p dir="ltr">
    <code> tls-server</code>
</p>
<p dir="ltr">
     <code>port 1194</code> #Change the port of OpenVPN to the one you want
</p>
<p dir="ltr">
     <code>proto tcp</code>
</p>
<p dir="ltr">
     <code>dev tun</code>
</p>
<p dir="ltr">
     <code>sndbuf 0</code>
</p>
<p dir="ltr">
     <code>rcvbuf 0</code>
</p>
<p dir="ltr">
     <code>ca ca.crt</code>
</p>
<p dir="ltr">
     <code>cert server.crt</code>
    <br/>
     <code>key server.key</code>
</p>
<p dir="ltr">
     <code>dh dh2048.pem</code>
</p>
<p dir="ltr">
     <code>tls-auth ta.key 0</code>
</p>
<p dir="ltr">
     <code>topology subnet</code>
</p>
<p dir="ltr">
     <code>server 10.8.0.0 255.255.255.0</code>
</p>
<p dir="ltr">
    <code> ifconfig-pool-persist ipp.txt</code>
</p>
<p dir="ltr">
     <code>push "redirect-gateway def1 bypass-dhcp"</code>
</p>
<p dir="ltr">
    #You can change the DNS you will be using to anyone you want
</p>
<p dir="ltr">
     <code>push "dhcp-option DNS 208.67.222.222"</code>
</p>
<p dir="ltr">
     <code>push "dhcp-option DNS 208.67.220.220"</code>
</p>
<p dir="ltr">
     <code>keepalive 10 120</code>
</p>
<p dir="ltr">
     <code>cipher AES-256-CBC</code>
</p>
<p dir="ltr">
    <code> comp-lzo</code>
</p>
<p dir="ltr">
    <code> persist-key</code>
</p>
<p dir="ltr">
    <code> persist-tun</code>
</p>
<p dir="ltr">
    <code> status openvpn-status.log</code>
</p>
<p dir="ltr">
     <code>verb 3</code>
</p>
<ol start="8">
    <li dir="ltr">
        <p dir="ltr">
            Configure the network to allow OpenVPN server:
        </p>
    </li>
</ol>
<p dir="ltr">
    In this step, you will need to change the following values to the correct
    numbers
</p>
<p dir="ltr">
    - YOURSERVERIPADDRESS: The Public IP address of your server
</p>
<p dir="ltr">
    - OPENVPNPORT: The port you will use for the OpenVPN Server
</p>
<p dir="ltr">
    - OBFSPORT: The port you will use for shapeshifter-dispatcher
</p>
<ol>
    <li dir="ltr">
        <p dir="ltr">
            Allow IP forward, most of the Linux distributions will have IP
            Forwarding disabled for security reasons, but we will need to allow
            IP forward in order to allow the OpenVPN to function. By running
            the following command, we will check if IP Forwarding is enabled:
        </p>
    </li>
</ol>
<p dir="ltr">
    <code>sysctl net.ipv4.ip_forward</code>
</p>
<p dir="ltr">
    if the value was 0
</p>
<p dir="ltr">
    <code>net.ipv4.ip_forward = 0</code>
</p>
<p dir="ltr">
    that means IP forward is disabled, to enable it run the following command:
</p>
<p dir="ltr">
    <code>echo 1 &gt; /proc/sys/net/ipv4/ip_forward</code>
</p>
<p>
    <strong>
        <br/>
    </strong>
</p>
<ol start="2">
    <li dir="ltr">
        <p dir="ltr">
            Set NAT for the VPN subnet:
        </p>
    </li>
</ol>
<p dir="ltr">
    <code>iptables -t nat -A POSTROUTING -s 10.8.0.0/24 -j SNAT --to
    YOURSERVERIPADDRESS</code>
</p>
<p dir="ltr">
    <code>iptables -I INPUT -p tcp --dport OPENVPNPORT -j ACCEPT</code>
</p>
<p dir="ltr">
    <code>iptables -I FORWARD -s 10.8.0.0/24 -j ACCEPT</code>
</p>
<p dir="ltr">
    <code>iptables -I FORWARD -m state --state RELATED,ESTABLISHED -j ACCEPT</code>
</p>
<p>
    <strong>
        <br/>
    </strong>
</p>
<ol start="3">
    <li dir="ltr">
        <p dir="ltr">
            Now you can start OpenVPN service by running the following
            commands:
        </p>
    </li>
</ol>
<p dir="ltr">
    <code>/etc/init.d/openvpn restart</code>
</p>
<p dir="ltr">
    <code>systemctl start openvpn@server</code>
</p>
<p>
    <strong>
        <br/>
    </strong>
</p>
<p dir="ltr">
    Now check if OpenVPN is running by checking if the tun0 interface is
    available
</p>
<p dir="ltr">
    <code>ip addr show tun0</code>
</p>
<p dir="ltr">
    if you see a configured interface (Assigned IP address to tun) run the
    following command to enable OpenVPN:
</p>
<p dir="ltr">
    <code>systemctl enable openvpn@server</code>
</p>
<ol start="9">
    <li dir="ltr">
        <p dir="ltr">
            Add client:
        </p>
    </li>
</ol>
<p dir="ltr">
    In this step, you will create the key, certificate and configuration files
    for your first OpenVPN user. The configuration file will have the client
    certificate embedded.
</p>
<p dir="ltr">
    <code>cd ~/openvpn-ca/</code>
</p>
<p dir="ltr">
   <code> source ./vars</code>
</p>
<p dir="ltr">
    Create a key file named CLIENT
</p>
<p dir="ltr">
    <code>./pkitool CLIENT</code>
</p>
<p dir="ltr">
    This process will generate CLIENT.key and CLIENT.crt
</p>
<p dir="ltr">
    Move them to OpenVPN folder
</p>
<p dir="ltr">
   <code> cd keys/</code>
</p>
<p dir="ltr">
  <code>  cp CLIENT.key CLIENT.crt /etc/openvpn</code>
</p>
<p dir="ltr">
    Create the client configuration file:
</p>
<p dir="ltr">
   <code> nano CLIENT.ovpn</code>
</p>
<p dir="ltr">
    Then add the following configurations to the file
</p>
<p dir="ltr">
   <code> client</code>
</p>
<p dir="ltr">
  <code>  dev tun</code>
</p>
<p dir="ltr">
   <code> proto tcp</code>
</p>
<p dir="ltr">
   <code> sndbuf 0</code>
</p>
<p dir="ltr">
   <code> rcvbuf 0</code>
</p>
<p dir="ltr">
   <code> remote YOURSERVERIPADDRESS </code>#Change this one to your public IP address
</p>
<p dir="ltr">
    <code>port 1194</code> #Change this one to the port you’re using for OpenVPN
</p>
<p dir="ltr">
   <code> resolv-retry infinite</code>
</p>
<p dir="ltr">
   <code> nobind</code>
</p>
<p dir="ltr">
   <code> persist-key</code>
</p>
<p dir="ltr">
   <code> persist-tun</code>
</p>
<p dir="ltr">
   <code> remote-cert-tls server</code>
</p>
<p dir="ltr">
   <code> cipher AES-256-CBC</code>
</p>
<p dir="ltr">
   <code> comp-lzo</code>
</p>
<p dir="ltr">
   <code> setenv opt block-outside-dns</code>
</p>
<p dir="ltr">
   <code> key-direction 1</code>
</p>
<p dir="ltr">
   <code> verb 3</code>
</p>
<p dir="ltr">
    <code>ls-auth ta.key 1</code>
</p>
<p dir="ltr">
    <code>ca ca.crt</code>
</p>
<p dir="ltr">
   <code> cert CLIENT.crt</code>
</p>
<p dir="ltr">
   <code> key CLIENT.key</code>
</p>
<p>
    <strong>
        <br/>
    </strong>
</p>
<p dir="ltr">
    Save the file, name it CLIENT.ovpn and put it it along with the following
    files ,CLIENT.crt, CLIENT.key, ta.key and ca.crt and test the OpenVPN
    connection.
</p>
<p dir="ltr">
    If it worked! Move forward to the installation of shapeshifter-dispatcher
    and enable obfuscation for OpenVPN.
</p>
<p>
    <strong>
        <br/>
    </strong>
</p>
<ol start="2">
    <li dir="ltr">
        <p dir="ltr">
            Shapeshifter-dispatcher server and client Installation:
        </p>
    </li>
</ol>
<p dir="ltr">
    In this step, we will install and use Shapeshifter-dispatcher Server and
    client on Ubuntu 16.04.1 LTS. Please note that the procedure will probably
    work on any Debian &amp; Ubuntu distro. You must run the the process as
    root or sudoers account.
</p>
<p dir="ltr">
    The installation process of Shapeshifter-dispatcher is the same on the
    server and client, so apply the steps 1,2,3 on both server and client.
</p>
<ol>
    <li dir="ltr">
        <p dir="ltr">
            you need to downloads the most updated package lists:
        </p>
    </li>
</ol>
<p dir="ltr">
   <code> apt-get update </code>
</p>
<ol start="2">
    <li dir="ltr">
        <p dir="ltr">
            In this step you will be installing the following packages that you
            will use during the installation and the operation stage:
        </p>
    </li>
</ol>
<ol>
    <ol>
        <li dir="ltr">
            <p dir="ltr">
                git, distributed version control system
            </p>
        </li>
        <li dir="ltr">
            <p dir="ltr">
                golang, Go programing language, which is needed to get
                shapeshifter-dispatcher to work
            </p>
        </li>
        <li dir="ltr">
            <p dir="ltr">
                curl, it’s a tool that will help you to get files from web or
                ftp server.
            </p>
        </li>
        <li dir="ltr">
            <p dir="ltr">
                Screen, this application will help you to run processes in the
                background
            </p>
        </li>
    </ol>
</ol>
<p dir="ltr">
   <code> apt-get install git golang curl screen -y </code>
</p>
<ol start="3">
    <li dir="ltr">
        <p dir="ltr">
            Install and configure shapeshifter-dispatcher, apply the following
            steps:
        </p>
    </li>
</ol>
<p dir="ltr">
   <code> mkdir ~/go</code>
</p>
<p dir="ltr">
  <code>  export GOPATH=~/go</code>
</p>
<p dir="ltr">
    <code>go get -u github.com/OperatorFoundation/shapeshifter-dispatcher/shapeshifter-dispatcher</code>
</p>
<ol start="4">
    <li dir="ltr">
        <p dir="ltr">
            Run shapeshifter-dispatcher on the server by running the following
            command:
        </p>
    </li>
</ol>
<p dir="ltr">
   <code> screen ~/go/bin/shapeshifter-dispatcher -server -transparent -ptversion 2
    -transports obfs2 -state state -bindaddr obfs2-YOURSERVERIPADDRESS:OBFSPORT
    -orport 127.0.0.1:OPENVPNPORT</code>
</p>
<p>
    <strong>
        <br/>
    </strong>
</p>
<p dir="ltr">
    You will get this screen. Don’t forget to change the IP address and ports
    within the command.
</p>
<p dir="ltr">
    <img
        src="https://lh3.googleusercontent.com/KuTZ5fE6A8Evxb7Px-BMyfSXW8Keq8hHFPBixAxkmeY4Wc1UtgBWYFOpN2YO6v3zao8bbqIGnGzTs3Ft3QQn2D_5Af4GBmQo28xWjLsjFtb7b7WMp0uZNPKRxfOAJ_wgWS9cNmLq"
        width="624"
        height="152"
        alt="Obfsservera.png"
    />
</p>
<ol start="5">
    <li dir="ltr">
        <p dir="ltr">
            Set NAT for shapeshifter-dispatcher on the server:
        </p>
    </li>
</ol>
<p dir="ltr">
   <code> iptables -I INPUT -p tcp --dport OBFSPORT -j ACCEPT</code>
</p>
<ol start="6">
    <li dir="ltr">
        <p dir="ltr">
            Run shapeshifter-dispatcher on the client side:
        </p>
    </li>
</ol>
<p dir="ltr">
    <code>screen ~/go/bin/shapeshifter-dispatcher -client -transparent -ptversion 2
    -transports obfs2 -state state -target YOURSERVERIPADDRESS:OBFSPORT</code>
</p>
<p dir="ltr">
    You will get this screen
</p>
<p dir="ltr">
    <img
        src="https://lh5.googleusercontent.com/rrZdx5-OjrcmG9pu_lF1eM8xr-4L0SbN2JcLgZ09vcY79GvZtcT9OgfnZzteAdhjaASy70RtFcUog7aydb9acAMdUmjuC_s0BoNtiWV2Plb89XhQudaIAHimtiptGFRwC8dB9KL4"
        width="622"
        height="128"
        alt="obfsclient.png"
    />
</p>
<p>
    <strong>
        <br/>
    </strong>
</p>
<ol start="7">
    <li dir="ltr">
        <p dir="ltr">
           <strong id"opvpnclient_installation">OpenVPN client configuration file and shapeshifter-dispatcher:</strong>
        </p>
    </li>
</ol>
<p dir="ltr">
    After running both the server and the client side, shapeshifter-dispatcher
    will connect and handshake which will then establish the connection between
    both sides.
</p>
<p dir="ltr">
    Now you will be able to reconfigure the client’s OpenVPN configuration file
    to send OpenVPN requests and packets to the client shapeshifter-dispatcher
    which will then establish the obfuscated connection with
    shapeshifter-dispatcher server which will receive the obfuscated packets
    and forward the OpenVPN client connection to the OpenVPN Server.
</p>
<p dir="ltr">
    Shapeshifter-dispatcher uses the port 1234 on the client side by default
    which means that this is the port which OpenVPN will talk to on the client
    machine.
</p>
<p dir="ltr">
    You need to open the file CLIENT.ovpn and change the following values:
</p>
<p dir="ltr">
    <code>remote 127.0.0.1</code> #OpenVPN needs to talk to the client
    Shapeshifter-dispatcher
</p>
<p dir="ltr">
   <code>port 1234</code> #OpenVPN needs to talk to Shapeshifter-dispatcher local port
</p>
<p dir="ltr">
    Save the file and establish the connection, you’re now off the radar!
</p>
<p>
    <strong>
        <br/>
        <br/>
    </strong>
</p>
<ol start="3">
    <li dir="ltr">
        <p dir="ltr">
            Conclusion
        </p>
    </li>
</ol>
<p dir="ltr">
    VPNs are user friendly and easy to manager service being used by users in
    countries with censorship which made it a target to government firewalls.
    Pluggable transport protocols made it possible to bypass such filtering
    without modifying the VPN itself but proxying the traffic into obfuscated
    tunnel and make the requests pass.
</p>
<p>
    <strong>
        <br/>
    </strong>
</p>
<p dir="ltr">
    Shapeshifter working as an independent service or a library to be added to
    your circumvention tool or connection will help users suffering from DPI
    censorship. This applies to other services, including but not limited to
    VOIP and SSH or any other UDP or TCP connection. Finally, a full
    integration between security tools and Pluggable transports is the is the
    way to go.
</p>
<p dir="ltr">
    For more information visit the following links:
</p>
<p>
    <strong>
        <br/>
    </strong>
</p>
