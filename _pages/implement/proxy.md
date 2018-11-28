---
layout: single
author_profile: false
permalink: /implement/proxy/

excerpt: ""
header:
  overlay_image: /assets/images/unsplash_0wdpet-ufqs-todd-diemer.jpg
  caption: "Photo credit: [**CC0 Unsplash / Todd Diemer**](https://unsplash.com/@todd_diemer)"
#  cta_label: "More Info"
#  cta_url: "https://unsplash.com"

sidebar:
    nav: "impnav"
    
---

**As a tool developer, the fastest way to get up and running is by using [Dispatcher](https://github.com/OperatorFoundation/shapeshifter-dispatcher), a tool developed by [The Operator Foundation](https://operatorfoundation.org/).** Dispatcher currently supports the following proxy modes:

* Transparent TCP
* Transparent UDP
* SOCKS5 (with optional PT 1.0 authentication protocol for tor)
* STUN UDP

Dispatcher supports multiple [obfuscation methods](/transports/), including meek, obfs4, shadowsocks, and scramblesuit.  It also supports obfs2 and obfs3, which should only be used for testing/educational exploration. 

For a sample walkthrough of using Dispatcher in this way, see the [OpenVPN Walkthrough](/implement/openvpn/)
 