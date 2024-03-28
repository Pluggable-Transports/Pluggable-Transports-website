---
layout: single
title: "Using Pluggable Transports"
author_profile: false
permalink: /implement/

excerpt: ""
header:
  overlay_image: /assets/images/Unsplash-7kkqg0eb_ti-ankush-minda.resized.jpg
  caption: "Photo credit: [**Unsplash / Ankush Minda**](https://unsplash.com/@an_ku_sh)"
#  cta_label: "More Info"
#  cta_url: "https://unsplash.com"

sidebar:
    nav: "impnav"

---

(Updated March 2024)

This table shows some examples of software already using Pluggable Transports across the most popular platforms: 

| --------------||
| **Desktop** | [Tor Browser](https://torproject.org), [Lantern](https://getlantern.org), [Psiphon](https://psiphon.ca), [RiseupVPN](https://riseup.net/en/vpn) |
| **Mobile - Android** | [Tor Browser](https://www.torproject.org/download/#android), [Briar Messaging](https://briarproject.org), [Lantern](https://getlantern.org), [Orbot](https://guardianproject.info/apps/orbot/), [Psiphon](https://psiphon.ca), [Calyx VPN](https://calyx.net), [RiseupVPN](https://riseup.net/en/vpn) |
| **Mobile - iOS** | [OnionBrowser](https://itunes.apple.com/us/app/onion-browser-secure-anonymous-web-with-tor/id519296448?mt=8), [Psiphon](https://itunes.apple.com/bm/app/psiphon/id1276263909), [Psiphon Browser](https://itunes.apple.com/ca/app/psiphon-browser/id1193362444?mt=8) |

{% include toc icon="file-text" %}

If you're a software developer looking to deploy Pluggable Transports, take a look at our How to Write a Pluggable Transport guide [here](/build/how).

## Implementation Options

If you just want to add existing transports into your application, this table summarizes the libraries that have already been developed and are available for us:

| --------------||
| **Desktop** | [PT 2.1 Go API](/implement/go/), [PT 3.0 Go API](https://github.com/Pluggable-Transports/Pluggable-Transports-spec/blob/main/releases/PTSpecV3.0/Pluggable%20Transport%20Specification%20v3.0%20-%20Go%20Transport%20API%20v3.0.md), [PT 3.0 Swift API](https://github.com/Pluggable-Transports/Pluggable-Transports-spec/blob/main/releases/PTSpecV3.0/Pluggable%20Transport%20Specification%20v3.0%20-%20Swift%20Transport%20API%20v3.0.md), [PT 3.0 Java API](https://github.com/Pluggable-Transports/Pluggable-Transports-spec/blob/main/releases/PTSpecV3.0/Pluggable%20Transport%20Specification%20v3.0%20-%20Java%20Transport%20API%20v1.0.md)
 |
| **Mobile - Android** | [PLUTO 2](https://github.com/guardianproject/AndroidPluggableTransports), [NetCipher](https://github.com/guardianproject/NetCipher)
| **Mobile - iOS** | [Swift API](https://github.com/Pluggable-Transports/Pluggable-Transports-spec/blob/master/releases/PTSpecV2.1Draft1/Pluggable%20Transport%20Specification%20v2.1%20-%20Swift%20Transport%20API%20v1.0%2C%20Draft%201.pdf) |


Implementation outside of these environments can be done with [Shapeshifter Dispatcher](/implement/shapeshifter), a command-line tool which provides a proxy that wraps the transport library. It has several different proxy modes and can proxy both TCP and UDP traffic.

The purpose of the dispatcher is to provide different proxy interfaces to using transports. Through the use of these proxies, application traffic can be sent over the network in a form that bypasses network filtering, allowing the application to work on networks where it would otherwise be blocked or heavily throttled.





