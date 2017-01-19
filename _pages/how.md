---
layout: single
title: "How obfuscation helps"
permalink: /how/

excerpt: ""
header:
  overlay_image: /assets/images/unsplash_Francisco Casero_empty-track.jpg
  caption: "Photo credit: [**CC0 Unsplash / Francisco Casero**](https://unsplash.com)"
#  cta_label: "More Info"
#  cta_url: "https://unsplash.com"

sidebar:
    nav: "sidenav"
---

# So you got blocked...

So your app’s server got blocked somewhere.  

## DNS Blocking

### What the host can do


### What users can do


### How this escalates

(Turkey example)


## IP based blocking

### What the host can do

You can swap in a new IP and push out an update, but the new IP will likely get blocked too.  Let's walk through how you and the people trying to access your app can try to overcome this blockage.

### What users can do

One quick trick that users might try is to use a VPN to get around the blockage -- so let’s take an example of an OpenVPN connection that uses the default configuration.

OpenVPN’s default port is 1194 and it uses SSL/TLS as a carrier for the encryption. A basic firewall -- like the one which is already blocking your app -- can easily classify a connection based on those elements then block the IP address or the domain of the OpenVPN server, this will prevent users from using the an OpenVPN default setup. 

</p>
                    <img
                        width="450"
                        height="84"
                        src="/assets/images/openVPN_packets.png"
                    />
                    <strong></strong>
                </p>
                <p>
                    <em>Figure (1) A wireshark capture for OpenVPN packets</em>
                </p>

    <strong id="how_pt_work" >How PT actually works</strong>
</p>
<p>
    <strong><em>a. </em></strong>
    <strong>
        <em>
            How the basic firewall can block traffic for a specific protocol?
        </em>
    </strong>
</p>
<p>
    <strong>Technically</strong>
    : to block the default setup of OpenVPN, a role of denying any connection
    through the port 1194 will be applied on the firewall, and this will
    prevent establishing a connection to the OpenVPN server.
</p>
<p>
</p>
<p>
    <strong>- A defense mechanism:</strong>
</p>
<p>

### How this escalates

In order to go around this kind of censorship, The VPN provider will change the port of the OpenVPN server to use the port
    443/SSL, and the client’s VPN connection will establish the connection to
    the port 443 which will make it hard to block the service by port number
    and it’s hard to filter real https traffic and differentiate it from the
    OpenVPN, both are encrypted.

The port 443 is the default port of SSL (https) protocol which represent
    the secure http connection and it’s hard to be blocked, 443 is essential to
    the Internet.


## Deep Packet Inspection


How can the Deep Packet Inspection block the non-default port
            connection of OpenVPN?

DPI inspects each packet based on the header of its request and the data it
    carries, it can identify the type of protocol or the connection is using
    even if it was encrypted. DPI is not a mechanism to decrypt what’s inside
    packets but to identify the ‘protocol’ or the application it represents.


This help the firewall to classify connections based on the content, then
    identify the IP address of the OpenVPN server and block the access.

    <img
        width="450"
        height="165"
        src="/assets/images/OpenVPNconnection.png"
    />

</p>
<p>
    <em>Figure (2) OpenVPN connection</em>


### What the host can do


### What users can do


### How this escalates


</p>
<p>
    <strong>What is Pluggable Transports then?</strong>
</p>
<p>
    <strong>Technically: </strong>
    PT is a tool that helps circumvent Internet censorship targeted by DPI,
    which allows a specific targeted protocol to bypass filtering and initiate
    a connection.
</p>
<p>
    On the other side, a recipient PT server will be waiting for the packets to
    arrive and then ‘Decipher’ them and then forward the packets to the OpenVPN
    server.
</p>
<p>
    The PT server is responsible of ‘ciphering’ the ongoing packets and push
    them to the client which will then ‘Decipher’ the packets and forward them
    to the OpenVPN client.
</p>
<p>
    <img
        width="382"
        height="197"
        src="/assets/images/DPIOpenVPN.png"
    />
</p>
<p>
    <em>Figure (3) DPI enabled on OpenVPN</em>
</p>
<p>
    DPI makes it harder for DPI to classify the connection and take an action
    against it
</p>
<p>
    <strong><em>To be added</em></strong>
</p>
<p>
    <em>Figure (2) A wireshark capture for Obfs4/OpenVPN packets</em>
</p>




You could also encrypt the traffic so it doesn’t look like it’s your app : Rot13 -> handshake -> easily blocked via handshake or IP once detected the first time

Let’s fix the handshake -> obfs2 (?) - still easily detected traffic, IP blocking

Let’s make the traffic look like noise -> obfs3 -> follow-up scanning, IP blocking

Let’s make the traffic harder -> obfs4 -> whitelisting traffic, 

Let’s make the traffic look like something else! -> FTE/Dust/Stego -> more resource-intense follow-up scanning, IP blocking

Let’s route through an allowed service -> fronting/meek -> throttling/temporary blocking

Let’s use ephemeral services -> flashproxy/snowflake -> challenges in having enough peers / reliable and getting info out to blocked users

… there’s no perfect solution, but this is where we are today. Not every adversary seeking to limit access to information can or is willing to do all of the above, so creative application of these, building new versions, innovation around new approaches -- and sharing of working options -- allow app and service providers to ensure connectivity through everything but complete shutdowns. 


<!-- 
<p>
    <strong>Metaphor: </strong>
    Let us assume that OpenVPN packets are represented physically as cubes with
    equal dimensions, the firewall is the guard who stands on the gate number
    1194 which represents the port and he has an order to block the movement of
    any cubes through the gate number 1194. This will prevent the red cubes of
    passing through the gate.
    
    <strong>Metaphor: </strong>
    The car that carries the cubes received an order to pass through the gate
    number 443 where the receiver on the other side of the gate is waiting,
    cubes will be able to pass with no issue because the gate 443 is a busy
    gate and it’s essential to the Internet and it will be problematic to block
    it.
    
    <strong>Metaphor</strong>
    : DPI is a more advanced guard that has the ability to identify ‘cubes’ and
    categories them regardless of the gate they’re going through. The
    Identification process is based on blocking certain type color, size and
    locks of the of the ‘cubes’. In this case it’s easy to prevent red colored
    ‘cubes’ from passing the gate.

    <strong>Metaphor</strong>
    : PT is a way to obfuscate the guards by changing the preferences of what
    we called ‘cubes’ by changing the color, size, and the used locks, this can
    happen by placing the ‘cubes’ inside another unknown type of ‘cubes’, send
    them over the car that waiting outside the gate, unpack them by following
    the same order they were packed at.
</p>
-->