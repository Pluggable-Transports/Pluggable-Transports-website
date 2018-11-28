---
layout: single
title: "What Pluggable Transports do"
permalink: /how2/

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

Pluggable Transports have been created to help developers keep their users connected when censorship occurs. In this article, we talk briefly about the ways in which transports can help. At this point, you should have already read our piece on [Types of censorship](/how/).

----------

## How Transports Help

Let us now walk through some of the options for transports, and how they vary in their approaches.  We'll start with a very simplistic approach:

### Simple obfuscation

One could easily "encrypt" traffic so it doesn’t look like the traffic that is being blocked. Using a Caesar cipher like [ROT13](https://en.wikipedia.org/wiki/ROT13), or minimal tweaks to parts of the traffic which are being used to identify it are examples - and have even worked for a while in some cases.

However, this is easily blocked by the adversary re-identifying the handshake and blocking based on that.

### Making it look like noise

A significant step up from this is to work towards making the traffic look like nothing at all. The goal here is to eliminate any identifying characteristics of the traffic, forcing a censor towards more expensive attacks.  This is the approach in Tor's most famous "obfs" line of transports, and we refer to this as [Scrambling](/transports/#scrambling).

To defeat scrambling, censors must either select only recognized and "approved" traffic out (whitelisting), scan the destination of each unrecognized stream of traffic and determine if it is legitimate or a circumvention tool, or find ways to interfere with or still identify the traffic.

China is famous for their "active probing" , and Iran for their willingness to terminate encrypted traffic after one minute. Kazakhstan was found to be using a system which matches the [timing of the obfs4 handshake and prevents it from successfully completing](https://bugs.torproject.org/20348).

### Making it look like something else

For adversaries willing or able to effectively combat scrambling, another approach is transforming the traffic to look like known - and approved - traffic. This faces its own challenges, as it becomes an arms race to ensure that this "[shape-shifting](/transports/#shape-shifting)" is good enough to continuously fool the censor - not just as the traffic, but also at the end-points.  If, for example, you are shape-shifting your traffic into vanilla http, the server will need to respond like a real web server. [The Parrot is Dead](https://www.cs.utexas.edu/~shmat/shmat_oak13parrot.pdf) explains the challenges these protocols face.

However, using this approach can force even more resource-intense follow-up scanning (the censor must now determine if the server is "correct" or not).

### Hiding in the crowd

Another approach leverages large cloud providers which are socially or economically difficult to block.  Using [Fronting](/transports/#fronting), you can route traffic through an allowed service.  This would force an adversary to block an entire cloud provider - and every other service hosted in it. Circumvention tool providers have previously been able to use this kind of connection, but it is increasingly being locked down as the cloud providers seek to retain control over their services.

Some adversaries are willing to block entire cloud providers - but often only temporarily. The downside of this powerful technique for tool providers is that it can in some cases be overwhelmingly financially expensive to route traffic through these services.

Another variant of this approach is using many, short-lived ephemeral connections.  An initial foray into this was called flashproxy, which leveraged website visitors themselves as proxies for others via javascript.  [Snowflake](https://github.com/keroserene/snowflake) has been more successful, getting around many of the challenges flashproxy faced by using WebRTC.

----------

Now that you know how Pluggable Transports work, it's time for you to [Meet the Transports](/transports/)!

<p style="text-align:left;"><a href="/how/">&lt; Previous</a>
<span style="float:right;"><a href="/transports/">Next &gt;</a></span>
</p>