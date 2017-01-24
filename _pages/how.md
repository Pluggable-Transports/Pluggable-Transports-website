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


{% include toc title="" icon="file-text" %}

So your app’s server got blocked somewhere.  




# DNS Blocking

DNS blocking is one of the easiest and fastest ways to block a website.  It is achieved by forcing ISPs to essentially misdirect users looking for the blocked website to a blank page or a page explaining why a site is blocked. This uses DNS, the Domain Name System, which is a decentralized "phone book" for the internet that translates domain names to internet addresses.

A famous example of this is Turkey's [March 2014 blocking of social media](https://www.theguardian.com/world/2014/mar/21/turkey-blocks-twitter-prime-minister) - notably Twitter. 


## What the host can do

The only real options for the host against this censorship is to provide alternative addresses -- and somehow communicate them to users.

## What users can do

In the case of Turkey, users were able to override their ISP's misinformation by switching to using Google's public DNS servers at 8.8.8.8 and 8.8.4.4.

![DNS Graffitti in Turkey](/assets/images/turkey-dns.jpg)

## How this escalates

There are a limited number of public DNS servers, which can be blocked with [IP blocking](#ip-blocking) and DNS traffic outside of a network itself can be [blocked at the protocol level](#port-blocking), forcing users to accept the "wrong" information.

# IP blocking

IP blocking requires more effort from the censor, and can have unintended consequences. Instead of blocking the "name" of a location on the internet, IP blocking targets its specific address.  Alone this and the DNS blocking technique are easily circumvented, but become challenging in combination.

## What the host can do

The host of any blocked service can easily swap in a new IP and push out an update, but the new IP will likely get blocked too.

Depending on the nature of the blockage, a host can also use a content delivery network or other cloud service provider, where one IP (and in many cases, one DNS name) may "answer" for thousands of different services.  Blocking that would cause "collateral damage" -- those thousands of other resources would also be blocked.  This is a core concept being used by [Greatfire's Collateral Freedom campaign](https://en.greatfire.org/blog/2015/mar/collateral-freedom-and-not-so-great-firewall), where they host censored content in the Amazon cloud and on github.  

## What users can do

One quick trick that users might try is to use a VPN to get around the blockage -- so let’s take an example of an OpenVPN connection that uses the default configuration.

OpenVPN’s default port is 1194 and it uses SSL/TLS as a carrier for the encryption. A basic firewall -- like the one which is already blocking your app -- can easily classify a connection based on those elements then block the IP address or the domain of the OpenVPN server, this will prevent users from using the an OpenVPN default setup. 

<img src="/assets/images/openVPN_packets.png" alt="A wireshark capture for OpenVPN packets" />

*A wireshark capture for OpenVPN packets*

## How this escalates

The collateral freedom approach is not without its own risks.  In the Greatfire example, while github and the Amazon Cloud were not blocked outright due to the socio-economic impacts, they both paid a steep price for hosting censored content in the form of a [crippling denial of service attack](http://arstechnica.com/security/2015/04/ddos-attacks-that-crippled-github-linked-to-great-firewall-of-china/)

Less advanced adversaries can also take a further step on even the most basic firewalls, which is [Protocol and Port Blocking](#port-blocking).

# Port Blocking

Even basic firewalls can block traffic for a specific protocol. This is commonly seen on public/guest/conference wifi networks which only allow "web browsing" (e.g. port 80 ans 443 - http and https ports). Censors can easily target traffic and simply not allow it to pass on its default ports.  In the OpenVPN example, blocking port 1194 will prevent a client from establishing a connection to **any** OpenVPN server.

## What the host can do

In order to go around this kind of censorship, The VPN provider will change the port of the OpenVPN server to use the port 443, and the client’s VPN connection will establish the connection to the port 443 which will make it hard to block the service by port number and it’s hard to filter real https traffic and differentiate it from the OpenVPN, as both are encrypted.

As port 443 is the default port for secure web browsing (https), it is rarely blocked.

## What users can do

As the sophistication of the censor increases, the options for users to circumvent the censorship from only their side narrows. Here, users must seek out services which allow them to proxy their traffic through unblocked ports, which generally will require setup in advance.

## How this escalates

Taken individually, this and the above techniques are not impossible for a dedicated user to circumvent, with some support from a host service.  However, they are easily combined to become more challenging -- a host may not only block the OpenVPN port, but also block access to know VPN websites via DNS, and known VPN hosts via IP blocking. 

Some adversaries may be willing to accept some of the downsides, including steep [economic costs](https://www.brookings.edu/research/internet-shutdowns-cost-countries-2-4-billion-last-year/), and accept blocking large portions of the internet or shutting external communications down completely for limited periods of time.

More advanced adversaries can move on from these to using a more active approach to censorship (and surveillance) - [Deep Packet Inspections, or DPI](#dpi-blocking).

# DPI Blocking

Deep Packet Inspections, or DPI inspects each packet based on the header of its request and the data it carries, it can identify the type of protocol or the connection is using even if it was encrypted. DPI is not a mechanism to decrypt what’s inside packets but to identify the ‘protocol’ or the application it represents.

Even if you are using OpenVPN on the https port, and not using a well-known VPN host (which could be blocked by IP and/or DNS), DPI can inspect the traffic and identify it as OpenVPN, and block it based on that inspection.

<img src="/assets/images/OpenVPNconnection.png" alt="An OpenVPN connection" />

## What the host can do


PT is a tool that helps circumvent Internet censorship targeted by DPI, which allows a specific targeted protocol to bypass filtering and initiate a connection.

On the other side, a recipient PT server will be waiting for the packets to arrive and then ‘Decipher’ them and then forward the packets to the OpenVPN server.

The PT server is responsible of ‘ciphering’ the ongoing packets and push them to the client which will then ‘Decipher’ the packets and forward them to the OpenVPN client.

<img src="/assets/images/DPIOpenVPN.png" alt="DPI blocking on OpenVPN" />

*DPI blocking on OpenVPN*

DPI makes it harder for DPI to classify the connection and take an action against it. [A Child's Garden on Pluggable Transports](https://trac.torproject.org/projects/tor/wiki/doc/AChildsGardenOfPluggableTransports) provides a step-by step walkthrough of how The Tor Project obfuscated tor network traffic, and how early obfuscation approaches were identified and blocked.

<!-- **Figure (2) A wireshark capture for Obfs4/OpenVPN packets** -->


## What users can do


## How this escalates

https://turkeyblocks.org/2016/12/18/tor-blocked-in-turkey-vpn-ban/ 



# Advanced Blocking



You could also encrypt the traffic so it doesn’t look like it’s your app : Rot13 -> handshake -> easily blocked via handshake or IP once detected the first time


Let’s fix the handshake -> obfs2 (?) - still easily detected traffic, IP blocking

Let’s make the traffic look like noise -> obfs3 -> follow-up scanning, IP blocking

Let’s make the traffic harder -> obfs4 -> whitelisting traffic, 

KZ: https://trac.torproject.org/projects/tor/ticket/20348#comment:184

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