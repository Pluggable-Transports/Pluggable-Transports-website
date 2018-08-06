---
layout: single
title: "How obfuscation helps"
permalink: /how/

excerpt: ""
header:
  overlay_image: /assets/images/SunsetTrainTracksCrop_Arne Hückelheim_notify_wikimedia.JPG
  caption: "Photo credit: [**© Arne Hückelheim**](https://en.wikipedia.org/wiki/User:Knipptang)"
#  cta_label: "More Info"
#  cta_url: "https://unsplash.com"

sidebar:
    nav: "sidenav"
---

{% include toc title="" icon="file-text" %}

Censorship is on the rise worldwide. This page describes some of the more common ways that websites and apps are blocked, and how both service providers and users can get around them. If you want to dive deeper into this subject, you should also take a look at [this IETF draft paper](https://github.com/josephlhall/rfc-censorship-tech/blob/master/as-submitted/draft-hall-censorship-tech-02.txt), published in May 2018.

----------


## Quick View

This table shows a summary of the information contained below. Click on the method to find more information and examples.

| **Method** | **Censor actions** | **Host actions** | **User actions** | 
| [DNS Blocking](#dns-blocking)| Forcing ISPs to misdirect users when certain URLs are accessed. | Provide and communicate alternative addresses. | Override the default DNS setting with a known safe one. |
| [IP Blocking](#ip-blocking) | Specific IP addresses are blocked. | Swap in a new IP address whenever the existing one is blocked, distribute proxy software. | Use a VPN, proxy or circumvention apps. |
| [Port Blocking](#port-blocking) | Stop traffic from accessing standard ports (eg OpenVPN on port 1194). | Use a different port, eg 443 which is usually used for https browsing. | Use more sophisticated VPN, proxy or circumvention apps. |
| [DPI Blocking](#dpi-blocking) | Target services by filtering packets, analyzing them, and dropping unwanted traffic. | Obfuscation becomes a requirement, eg using Pluggable Transports. | Seek apps and software that are designed to evade censorship.

----------

## DNS Blocking

DNS blocking is one of the easiest and fastest ways to block a website.  It is achieved by forcing ISPs to essentially misdirect users looking for the blocked website to a blank page or a page explaining why a site is blocked. This uses DNS, the Domain Name System, which is a decentralized "phone book" for the internet that translates domain names to internet addresses.

A famous example of this is Turkey's [March 2014 blocking of social media](https://www.theguardian.com/world/2014/mar/21/turkey-blocks-twitter-prime-minister) - notably Twitter. 

#### What the host can do

The only real options for the host against this censorship is to provide alternative addresses -- and somehow communicate them to users.

#### What users can do

In the case of Turkey, users were able to override their ISP's misinformation by switching to using Google's public DNS servers at 8.8.8.8 and 8.8.4.4.

![DNS Graffitti in Turkey](/assets/images/turkey-dns.jpg)

#### How this escalates

There are a limited number of public DNS servers, which can be blocked with [IP blocking](#ip-blocking) and DNS traffic outside of a network itself can be [blocked at the protocol level](#port-blocking), forcing users to accept the "wrong" information.

## IP blocking

IP blocking requires more effort from the censor, and can have unintended consequences. Instead of blocking the "name" of a location on the internet, IP blocking targets its specific address.  When used alone, both this and the DNS blocking technique are easily circumvented, but become more challenging in combination.

#### What the host can do

The host of any blocked service can easily swap in a new IP and push out an update, but the new IP will likely get blocked too.

Depending on the nature of the blockage, a host can also use a content delivery network or other cloud service provider, where one IP (and in many cases, one DNS name) may "answer" for thousands of different services.  Blocking that would cause "collateral damage" -- those thousands of other resources would also be blocked. Alternatively, proxy servers and apps can be pushed out to users of the service, connecting directly to the service and hiding the final address from the censor.

#### What users can do

One quick trick that users might try is to use a VPN to get around the blockage -- so let’s take an example of an OpenVPN connection that uses the default configuration.

OpenVPN’s default port is 1194 and it uses SSL/TLS as a carrier for the encryption. A basic firewall -- like the one which is already blocking your app -- can easily classify a connection based on those elements then block the IP address or the domain of the OpenVPN server, this will prevent users from using the an OpenVPN default setup.

Other VPN-like services are also available, such as [Lantern](https://getlantern.org), [Psiphon](https://www.psiphon3.com) and [TunnelBear](https://www.tunnelbear.com). These use more sophisticated techniques to allow end users to tunnel through network interference to the desired service.

<img src="/assets/images/openVPN_packets.png" alt="A wireshark capture for OpenVPN packets" />

*A wireshark capture for OpenVPN packets*

#### How this escalates

The collateral freedom approach is not without its own risks.  In one well-known case, github and the Amazon Cloud were unknowingly being used for collateral freedom, and while they were not blocked outright, likely due to the socio-economic impacts, they both paid a steep price for hosting censored content in the form of a [crippling denial of service attack](http://arstechnica.com/security/2015/04/ddos-attacks-that-crippled-github-linked-to-great-firewall-of-china/)

Less advanced adversaries can also take a further step on even the most basic firewalls, which is [Protocol and Port Blocking](#port-blocking).

## Port Blocking

Even basic firewalls can block traffic for a specific protocol. This is commonly seen on public/guest/conference wifi networks which only allow "web browsing" (e.g. port 80 ans 443 - http and https ports). Censors can easily target traffic and simply not allow it to pass on its default ports.  In the OpenVPN example, blocking port 1194 will prevent a client from establishing a connection to **any** OpenVPN server.

#### What the host can do

In order to go around this kind of censorship, The VPN provider will change the port of the OpenVPN server to use the port 443, and the client’s VPN connection will establish the connection to the port 443 which will make it hard to block the service by port number and it’s hard to filter real https traffic and differentiate it from the OpenVPN, as both are encrypted.

As port 443 is the default port for secure web browsing (https), it is rarely blocked.

#### What users can do

As the sophistication of the censor increases, the options for users to circumvent the censorship from only their side narrows. Here, users must seek out services which allow them to proxy their traffic through unblocked ports, which generally will require setup in advance.

#### How this escalates

Taken individually, this and the above techniques are not impossible for a dedicated user to circumvent, with some support from a host service.  However, they are easily combined to become more challenging -- a host may not only block the OpenVPN port, but also block access to know VPN websites via DNS, and known VPN hosts via IP blocking. 

Some adversaries may be willing to accept some of the downsides, including steep [economic costs](https://www.brookings.edu/research/internet-shutdowns-cost-countries-2-4-billion-last-year/), and accept blocking large portions of the internet or shutting external communications down completely for limited periods of time.

More advanced adversaries can move on from these to using a more active approach to censorship (and surveillance) - [Deep Packet Inspections, or DPI](#dpi-blocking).

## DPI Blocking

Deep Packet Inspection (DPI) inspects each packet based on the header of its request and the data it carries. It can identify the type of protocol the connection is using even if it was encrypted. DPI is not a mechanism to decrypt what is inside packets but to identify the ‘protocol’ or the application it represents.

With DPI, a censor can target VPN, Tor and other services by filtering the packets at the Application Layer of the OSI reference model, analyze them, and then block the IP addresses that the clients are trying to connect to.

Even if you are using OpenVPN on the https port, and not using a well-known VPN host (which could be blocked by IP and/or DNS), DPI can inspect the traffic and identify it as OpenVPN, and block it based on that inspection.

|**A Short History of DPI Blocking**|
|In 2012, both [Iran](https://smallmedia.org.uk/work/writers-block-the-story-of-censorship-in-iran) and [China](https://www.technologyreview.com/s/427413/how-china-blocks-the-tor-anonymity-network/) began filtering encrypted internet traffic, specifically targeting The Tor Project's anti-censorship, pro-privacy network; triggering The Tor Project to release their first public [obfuscation work](https://blog.torproject.org/blog/obfsproxy-next-step-censorship-arms-race).|
|Other countries soon applied the same strategy -- [Ethiopia](https://blog.torproject.org/blog/ethiopia-introduces-deep-packet-inspection) later in 2012, [Syria](http://secdev-foundation.org/wp-content/uploads/2014/08/Flash-Note-Syria-8-Syrian-regime-tightens-access-to-secure-online-communications.pdf) in 2014, among many others. Earlier this year, the OONI project wrote about DPI being used in Iran [to block Instagram](https://ooni.torproject.org/post/2018-iran-protests-pt2/). Privacy International tracks surveillance systems in their [Transparency Toolkit](https://sii.transparencytoolkit.org/); and Freedom House's [Freedom on the Net](https://freedomhouse.org/report/freedom-net/freedom-net-2017) tracks both censorship and other limitations on net freedom.|


#### What the host can do

**At this stage, obfuscation of traffic becomes a requirement** - to evade DPI and thereby protect servers from being immediately [blocked via IP](#ip-blocking). Pluggable Transports were first developed by [The Tor Project](https://www.torproject.org/docs/pluggable-transports.html.en) to provide ways for tool developers to make their apps more resilient against DPI filtering.

Pluggable Transports make it harder for DPI to classify the connection and take an action against it. [A Child's Garden of Pluggable Transports](https://trac.torproject.org/projects/tor/wiki/doc/AChildsGardenOfPluggableTransports) provides a step-by step walkthrough of how The Tor Project obfuscated tor network traffic, and how early obfuscation approaches were identified and blocked.

<!-- 
On the other side, a recipient PT server will be waiting for the packets to arrive and then ‘Decipher’ them and then forward the packets to the OpenVPN server.

The PT server is responsible of ‘ciphering’ the ongoing packets and push them to the client which will then ‘Decipher’ the packets and forward them to the OpenVPN client.

<img src="/assets/images/DPIOpenVPN.png" alt="DPI blocking on OpenVPN" />

*DPI blocking on OpenVPN* 
-->

<!-- **Figure (2) A wireshark capture for Obfs4/OpenVPN packets** -->


#### How this escalates

In December 2016, Turkey escalated their censorship abilities from DNS blocking to DPI-based blocking, focusing on [censoring VPN and Tor traffic](https://turkeyblocks.org/2016/12/18/tor-blocked-in-turkey-vpn-ban/).

Pluggable Transports continued to work despite this escalation. 

## Advanced Censorship

Let us now walk through some of the options for transports, and how they vary in their approaches.  We'll start with a very simplistic approach:

#### Simple obfuscation

One could easily "encrypt" traffic so it doesn’t look like the traffic that is being blocked.  Using a Caesar cipher like [ROT13](https://en.wikipedia.org/wiki/ROT13), or minimal tweaks to parts of the traffic which are being used to identify it are examples - and have even worked for a while in some cases.

However, this is easily blocked by the adversary re-identifying the handshake and blocking based on that.

#### Making it look like noise

A significant step up from this is to work towards making the traffic look like nothing at all. The goal here is to eliminate any identifying characteristics of the traffic, forcing a censor towards more expensive attacks.  This is the approach in Tor's most famous "obfs" line of transports, and we refer to this as [Scrambling](/transports/#scrambling).

To defeat scrambling, censors must either select only recognized and "approved" traffic out (whitelisting), scan the destination of each unrecognized stream of traffic and determine if it is legitimate or a circumvention tool, or find ways to interfere with or still identify the traffic.

China is famous for their "active probing" , and Iran for their willingness to terminate encrypted traffic after one minute. Kazakhstan was found to be using a system which matches the [timing of the obfs4 handshake and prevents it from successfully completing](https://bugs.torproject.org/20348).

#### Making it look like something else

For adversaries willing or able to effectively combat scrambling, another approach is transforming the traffic to look like known - and approved - traffic. This faces its own challenges, as it becomes an arms race to ensure that this "[shape-shifting](/transports/#shape-shifting)" is good enough to continuously fool the censor - not just as the traffic, but also at the end-points.  If, for example, you are shape-shifting your traffic into vanilla http, the server will need to respond like a real web server. [The Parrot is Dead](https://www.cs.utexas.edu/~shmat/shmat_oak13parrot.pdf) explains the challenges these protocols face.

However, using this approach can force even more resource-intense follow-up scanning (the censor must now determine if the server is "correct" or not).

#### Hiding in the crowd

Another approach leverages large cloud providers which are socially or economically difficult to block.  Using [Fronting](/transports/#fronting), you can route traffic through an allowed service.  This would force an adversary to block an entire cloud provider - and every other service hosted in it. Circumvention tool providers have previously been able to use this kind of connection, but it is increasingly being locked down as the cloud providers seek to retain control over their services.

Some adversaries are willing to block entire cloud providers - but often only temporarily. The downside of this powerful technique for tool providers is that it can in some cases be overwhelmingly financially expensive to route traffic through these services.

Another variant of this approach is using many, short-lived ephemeral connections.  An initial foray into this was called flashproxy, which leveraged website visitors themselves as proxies for others via javascript.  [Snowflake](https://github.com/keroserene/snowflake) has been more successful, getting around many of the challenges flashproxy faced by using WebRTC.

## The Future

There is no one perfect solution, but this is where we are today.

Not every adversary seeking to limit access to information can, or is willing to, do all of the above, so creative application of these, building new versions, innovation around new approaches -- and sharing of working options -- allow app and service providers to ensure connectivity through everything but complete shutdowns.


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