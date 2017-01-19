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
There are many different transports which address a wide variety of blocking strategies. At a very high-level standpoint, they can be grouped into three categories: **[Fronting](#fronting)** , **[Scrambling](#scrambling)** , and **[Shape-Shifting](#shape-shifting)**.

## Fronting

Fronting approaches leverage cloud platforms which are socially or economically difficult to block - for example, allowing access to any Google services allows access to all Google services, including their app engine.  The same goes for Amazon Web Services and Azure.  To effectively block an app using fronting requires blocking the entire cloud provider -- and every other service hosted by it.

* **Where it works best**: Anywhere where blocking significant online platforms would be not tenable long-term due to desires to support innovation, economic linkages, or simply social expectations of access.

* **Downsides**: Using these services can quickly become very costly, and while powerful, still not a guarantee that the connections will not be throttled, interefered with, or blocked either temporarily or over longer terms.

* **Implementations** 

| Name | Description | Status |
|**[meek](https://trac.torproject.org/projects/tor/wiki/doc/meek)** | Meek is the gold standard and supports "fronting" through Google App Engine, Amazon Web Services and Microsoft Azure. | In use. [Evaluation](https://trac.torproject.org/projects/tor/wiki/doc/PluggableTransports/MeekEvaluation)|
|**[SnowFlake](https://keroserene.net/snowflake/)** | SnowFlake uses WebRTC to turn website visitors into ephemeral proxies. See also [torproject.org/projects/tor/wiki/doc/Snowflake](https://trac.torproject.org/projects/tor/wiki/doc/Snowflake) | alpha [Evaluation](https://trac.torproject.org/projects/tor/wiki/doc/PluggableTransports/SnowFlakeEvaluation) |
|**[Decoy Routing](https://www.decoyrouting.com/)** | Decoy Routing (also known as TapDance and Telex) is a concept in progress which intercepts traffic going to "safe" addresses and redirects it to user-requested, blocked content. See also https://github.com/SergeyFrolov/gotapdance | In Development. | 

Other options include using short-lived VPS servers and even dividing traffic among multiple, low-cost fronting options.

## Scrambling

This approach seeks to disguise the traffic in ways that are not identifiable as any specific type of traffic. This forces an adversary to only allowed whitelisted traffic (which can be easily defeated by fronting) or to expend significant resources to re-identify the scrambled traffic.

* **Where it works best**: 

* **Downsides**: 

* **Implementations**

| Name | Description | Status |


	* **[obfs4](https://github.com/Yawning/obfs4)**   [Evaluation](https://trac.torproject.org/projects/tor/wiki/doc/PluggableTransports/Obfs4Evaluation)
	* (Depricated) Obfs3
	* (Depricated) Obfs2

## Shape-Shifting

"Shapeshifting" hide the traffic in non-objectional formats, making it look like a VOIP call, web traffic, online games, or statistically sampled "normal" traffic.

* **Where it works best**: This approach defeats whitelisted traffic limitations

* **Downsides**: Shape-Shifting is almost impossible to do "perfectly" -- even if the traffic is indistinguishable from "real" traffic, the client and server will generally behave differently from a "real" server - meaning that advanced adversaries can choose to expend resources to do follow-up scans on every suspected connection to verify the server acts "correctly," and block it if not. 

* **Implementations**
	* **[Dust](https://github.com/blanu/Dust)**  
	* **[Stegotorus](https://github.com/TheTorProject/stegotorus)** 
	* **[FTEProxy](https://fteproxy.org/)** Format-Transforming Encryption Proxy uses regular expressions to transform blocked traffic (e.g. tor, https, VOIP) into traffic that appears to be allowed traffic (e.g. plain http) [Evaluation](https://trac.torproject.org/projects/tor/wiki/doc/PluggableTransports/FteEvaluation)
	* **SkypeMorph**
	* **BananaPhone**
