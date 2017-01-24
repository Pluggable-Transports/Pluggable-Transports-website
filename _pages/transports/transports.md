---
layout: single
title: Meet the Transports
permalink: /transports/

excerpt: ""
header:
  overlay_image: /assets/images/SunsetTrainTracksCrop_Arne Hückelheim_notify_wikimedia.JPG
  caption: "Photo credit: [**© Arne Hückelheim**](https://en.wikipedia.org/wiki/User:Knipptang)"
#  cta_label: "More Info"
#  cta_url: "https://unsplash.com"

sidebar:
    nav: "sidenav"
---
There are many different transports which address a wide variety of blocking strategies. At a very high-level standpoint, they can be grouped into three categories: 

{% include toc icon="file-text" %}

## Fronting

Fronting approaches leverage cloud platforms which are socially or economically difficult to block - for example, allowing access to any Google services allows access to all Google services, including their app engine.  The same goes for Amazon Web Services and Azure.  To effectively block an app using fronting requires blocking the entire cloud provider -- and every other service hosted by it.

* **Where it works best**: Anywhere where blocking significant online platforms would be not tenable long-term due to desires to support innovation, economic linkages, or simply social expectations of access.

* **Downsides**: Using these services can quickly become very costly, and while powerful, still not a guarantee that the connections will not be throttled, interefered with, or blocked either temporarily or over longer terms.

* **Implementations** 

| Name | Description | Status |
|**[meek](https://trac.torproject.org/projects/tor/wiki/doc/meek)** | Meek is the gold standard and supports "fronting" through Google App Engine, Amazon Web Services and Microsoft Azure. | In use. [Evaluation](https://trac.torproject.org/projects/tor/wiki/doc/PluggableTransports/MeekEvaluation)|
|**[SnowFlake](https://keroserene.net/snowflake/)** | SnowFlake uses WebRTC to turn website visitors into ephemeral proxies. See also [torproject.org/projects/tor/wiki/doc/Snowflake](https://trac.torproject.org/projects/tor/wiki/doc/Snowflake) | alpha [Evaluation](https://trac.torproject.org/projects/tor/wiki/doc/PluggableTransports/SnowFlakeEvaluation) |
|**[FlashProxy](https://crypto.stanford.edu/flashproxy/)**|Flashproxy leverages javascript running in uncensored browsers to provide short-lived proxies to censored content. Firewall traversal causes challenges for this, which are being addressed in SnowFlake, above. | Deprecated [Evaluation](https://trac.torproject.org/projects/tor/wiki/doc/PluggableTransports/FlashproxyEvaluation)| 
|**[Decoy Routing](https://www.decoyrouting.com/)** | Decoy Routing (also known as TapDance and Telex) is a concept in progress which intercepts traffic going to "safe" addresses and redirects it to user-requested, blocked content. See also https://github.com/SergeyFrolov/gotapdance | In Development. | 

Other options include using short-lived VPS servers and even dividing traffic among multiple, low-cost fronting options.

## Scrambling

This approach seeks to disguise the traffic in ways that are not identifiable as any specific type of traffic. This forces an adversary to only allowed whitelisted traffic (which can be easily defeated by fronting) or to expend significant resources to re-identify the scrambled traffic.

* **Where it works best**: Scrambling tends to work best where there is not only a willingness to outright block large platform providers, but also DPI analysis of internet traffic

* **Downsides**: While the infrastructure costs are not as large as fronting, using scrambling still relies on having clients being able to know or discover unblocked IP addresses, and an active censor will work to discover and block these addresses. Modern implementations (such as obfs4) implement defenses against this attack.

* **Implementations**

| Name | Description | Status |
|**[obfs4](https://github.com/Yawning/obfs4)**| Obfs4 is the current state of the art deployed "look-like nothing" obfuscation protocol from The Tor Project.  It builds off of ScrambleSuit, below.  **NOTE:** Obfs4 is being [successfully blocked in Kazakhstan](https://trac.torproject.org/projects/tor/ticket/20348)|Actively used, [Evaluation](https://trac.torproject.org/projects/tor/wiki/doc/PluggableTransports/Obfs4Evaluation)|
|**[ScrambleSuit](http://www.cs.kau.se/philwint/scramblesuit/)**| ScrambleSuit combines difficult to fingerprint unique traffic while also defending against "active probing" by adversaries | Superseded by Obfs4 [Evaluation](https://trac.torproject.org/projects/tor/wiki/doc/PluggableTransports/ScrambleSuitEvaluation)| 
|**[Obfs3](https://gitweb.torproject.org/pluggable-transports/obfsproxy.git/tree/doc/obfs3/obfs3-protocol-spec.txt)**| Obfs3 provides basic obfuscation but without the active probing defenses of Obfs4. | [Obfs3 Evaluation](https://trac.torproject.org/projects/tor/wiki/doc/PluggableTransports/Obfs3Evaluation)|
|**[Obfs2](https://gitweb.torproject.org/pluggable-transports/obfsproxy.git/tree/doc/obfs2/obfs2-protocol-spec.txt)**| Obfs2 was developed as an early protection against simple traffic identification, and does not defend against DPI analysis. To quote the evaluation, Obfs2 "is considered trivially breakable by most adversaries and is deprecated and has been evaluated as a historical reference only." |Deprecated [Obfs2 Evaluation](https://trac.torproject.org/projects/tor/wiki/doc/PluggableTransports/Obfs2Evaluation)  |

## Shape-Shifting

"Shapeshifting" hide the traffic in non-objectional formats, making it look like a VOIP call, web traffic, online games, or statistically sampled "normal" traffic.

* **Where it works best**: This approach defeats whitelisted traffic limitations

* **Downsides**: Shape-Shifting is almost impossible to do "perfectly" -- even if the traffic is indistinguishable from "real" traffic, the client and server will generally behave differently from a "real" server - meaning that advanced adversaries can choose to expend resources to do follow-up scans on every suspected connection to verify the server acts "correctly," and block it if not. 

* **Implementations**

|**[Dust2](https://github.com/blanu/Dust)**|Dust transforms traffic to statistically match a pattern of "allowed" traffic |[Evaluation](https://trac.torproject.org/projects/tor/wiki/doc/PluggableTransports/Dust2Evaluation)|
| **[Stegotorus](https://github.com/TheTorProject/stegotorus)**|||
|**[FTEProxy](https://fteproxy.org/)** | Format-Transforming Encryption Proxy uses regular expressions to transform blocked traffic (e.g. tor, https, VOIP) into traffic that appears to be allowed traffic (e.g. plain http) | [Evaluation](https://trac.torproject.org/projects/tor/wiki/doc/PluggableTransports/FteEvaluation)|


Visit [The Tor Project](https://trac.torproject.org/projects/tor/wiki/doc/PluggableTransports/list) for a list that includes additional theoretical transports and whitepapers.
