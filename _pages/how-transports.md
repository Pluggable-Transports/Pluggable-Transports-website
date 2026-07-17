---
layout: single
title: "What Pluggable Transports do"
permalink: /how-transports/

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

Pluggable Transports have been created to help developers keep their users connected when censorship occurs. In this article, we talk briefly about the ways in which transports can help. At this point, you should have already read our piece on [Types of censorship](/how/). Additionally, we recommend reading the [RFC 9505](https://datatracker.ietf.org/doc/rfc9505) for further background on current censorship techniques worldwide.

----------

## How Transports Help

Let us now walk through some of the options for transports, and how they vary in their approaches.  We'll start with a very simplistic approach:

### Simple obfuscation

One could easily "encrypt" traffic so it doesn’t look like the traffic that is being blocked. Using a Caesar cipher like [ROT13](https://en.wikipedia.org/wiki/ROT13), or minimal tweaks to parts of the traffic which are being used to identify it are examples - and have even worked for a while in some cases.

However, this is easily blocked by the adversary re-identifying the handshake and blocking based on that.

### Making it look like noise

A significant step up from this is to work towards making the traffic look like nothing at all. The goal here is to eliminate any identifying characteristics of the traffic, forcing a censor towards more expensive attacks.  This is the approach in Tor's most famous "obfs" line of transports, such as [obfs4](https://software.pluggabletransports.info/obfs4) which incorporates adaptive obfuscation patterns based on censorship levels, and we refer to this as [Scrambling](/transports/#scrambling).

To defeat scrambling, censors must either select only recognized and "approved" traffic out (whitelisting), scan the destination of each unrecognized stream of traffic and determine if it is legitimate or a circumvention tool, or find ways to interfere with or still identify the traffic.

China is famous for their "active probing" , and Iran for their willingness to terminate encrypted traffic after one minute. Kazakhstan was found to be using a system which matches the [timing of the obfs4 handshake and prevents it from successfully completing](https://bugs.torproject.org/20348). More recently, and as documented by Tor in this [issue](https://gitlab.torproject.org/tpo/applications/tor-browser/-/issues/41870), both obfs4 and snowflake continue facing blockings by the GFW of China, and although no current alternatives have been found to some specific timeouts, the reports are now a reference for potential future improvements using new tools such as [xray](https://cscot.pages.dev/2023/07/02/xray-reality-h2).

### Making it look like something else

For adversaries willing or able to effectively combat scrambling, another approach is transforming the traffic to look like known - and approved - traffic, with [Replicant](https://software.pluggabletransports.info/replicant), (which utilizes AI to continuously analyze and adapt to allowed traffic patterns) and Optimizer ,which employs various strategies to mimic legitimate traffic behaviors. This faces its own challenges, as it becomes an arms race to ensure that this "[shape-shifting](/transports/#shape-shifting)" is good enough to continuously fool the censor - not just as the traffic, but also at the end-points.  If, for example, you are shape-shifting your traffic into vanilla http, the server will need to respond like a real web server. [The Parrot is Dead](https://www.cs.utexas.edu/~shmat/shmat_oak13parrot.pdf) explains the challenges these protocols face.

Incorporating techniques like [web tunneling](https://blog.torproject.org/introducing-webtunnel-evading-censorship-by-hiding-in-plain-sight/), which disguises traffic as an HTTPS connection, can enhance this approach. Web tunneling wraps the payload in a WebSocket-like HTTPS connection, making it appear as ordinary HTTPS traffic to network observers and giving the impression of standard web browsing. In spite of these more recent developments in Shape-shifting strategies, the fact remains that using these types of mechanisms can force even more resource-intensive follow-up scanning since the censor must then determine if the server is "correct" or not, and teams and providers aiming to develop solutions with these strategies will need to overcome such barriers.

[V2Ray](https://www.v2ray.com/en/) is a tool designed for obfuscating internet traffic, featuring multiple built-in protocols, including [Shadowsocks](https://www.shadowsocks.org/), it offers different methods of camouflage to make the traffic appear as regular, non-censored communication.

 
### Hiding in the crowd

Another approach leverages large cloud providers which are socially or economically difficult to block.  Using [Fronting](/transports/#fronting), also known as Diverting, you can route traffic through an allowed service.  This would force an adversary to block an entire cloud provider - and every other service hosted in it. Circumvention tool providers have previously been able to use this kind of connection, but it is increasingly being locked down as the cloud providers seek to retain control over their services.

Some adversaries are willing to block entire cloud providers - but often only temporarily. The downside of this powerful technique for tool providers is that it can in some cases be overwhelmingly financially expensive to route traffic through these services, as showcased in this [article](https://www.sentinelone.com/blog/privacy-2019-tor-meek-rise-fall-domain-fronting/). In response, the circumvention community continues to innovate, exploring decentralized and peer-to-peer technologies to enhance resilience and reduce reliance on any single provider. These advancements reflect a dynamic and adaptive approach to overcoming censorship, ensuring that access to open and unrestricted internet remains available.

Another variant of this approach is using many, short-lived ephemeral connections.  An initial foray into this was called [flashproxy](https://crypto.stanford.edu/flashproxy/ ), which leveraged website visitors themselves as proxies for others via javascript.  [Snowflake](https://github.com/keroserene/snowflake) has been more successful, getting around many of the challenges flashproxy faced by using WebRTC, an HTML5 interface that provides real-time communication between browsers and devices. More recent developments include [Shadowsocks](https://software.pluggabletransports.info/shadowsocks), which uses a fast tunnel proxy to bypass firewalls, acting as a bridge in restricted networks, and [dnstt](https://software.pluggabletransports.info/dnstt), which demonstrates ephemeral bridges through its DNS tunneling capability, using DoH and DoT to frequently change and adapt to censorship.

Looking into the future
=======================

{Geneva (Genetic Evasion)}(https://geneva.cs.umd.edu/) This is a novel approach that uses genetic algorithms to discover new evasion strategies against internet censorship. While not a pluggable transport itself, Geneva's strategies can inform the development of new pluggable transports by automatically finding ways to manipulate packet streams to evade censorship.

With the advent of quantum computing, there's a growing emphasis on developing [quantum-resistant cryptographic](https://www.nist.gov/news-events/news/2022/07/nist-announces-first-four-quantum-resistant-cryptographic-algorithms) methods to ensure that traffic scrambling remains secure against future threats, enhancing the long-term viability of these techniques.

The rise of [decentralized technologies](https://cointelegraph.com/learn/types-of-daos), such as blockchain and decentralized autonomous organizations (DAOs), has led to the development of more distributed circumvention tools. These tools are harder to block or censor due to their lack of a single point of failure.

----------

Now that you know how Pluggable Transports work, it's time for you to [Meet the Transports](/transports/)!

<p style="text-align:left;"><a href="/how/">&lt; Previous</a>
<span style="float:right;"><a href="/transports/">Next &gt;</a></span>
</p>
