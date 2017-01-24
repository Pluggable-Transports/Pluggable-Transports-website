---
layout: single
author_profile: false
permalink: /implement/

excerpt: ""
header:
  overlay_image: /assets/images/Unsplash-7kkqg0eb_ti-ankush-minda.resized.jpg
  caption: "Photo credit: [**Unsplash / Ankush Minda**](https://unsplash.com/@an_ku_sh)"
#  cta_label: "More Info"
#  cta_url: "https://unsplash.com"

sidebar:
    nav: "sidenav"
---

**As a tool developer, the fastest way to get up and running is by using [Dispatcher](https://github.com/OperatorFoundation/shapeshifter-dispatcher), a tool developed by [The Operator Foundation](https://operatorfoundation.org/).**

Dispatcher currently supports the following proxy modes:

* SOCKS5 (with optional PT 1.0 authentication protocol for tor)
* Transparent TCP
* Transparent UDP
* STUN UDP

Dispatcher supports multiple [obfuscation methods](/transports/), including meek, obfs4, and scramblesuit.  It also supports obfs2 and obfs3, which should only be used for testing/educational exploration. 

## What is Dispatcher?

There are two components: transports and the dispatcher. Each transport provide different approach to obfuscation. These transports are provided as a Go library which can be integrated directly into applications. The dispatcher is a command line tool which provides a proxy that wraps the transport library. It has several different proxy modes and can proxy both TCP and UDP traffic.

If you are a tool developer working in the Go programming language, then you probably want to use the [transports library](https://github.com/OperatorFoundation/shapeshifter-transports)  directly in your application. 

If you want a end user that is trying to circumvent filtering on your network or you are a developer that wants to add pluggable transports to an existing tool that is not written in the Go programming language, then you probably want the dispatcher. Please note that familiarity with executing programs on the command line is necessary to use this tool. 

The purpose of the dispatcher is to provide different proxy interfaces to using transports. Through the use of these proxies, application traffic can be sent over the network in a form that bypasses network filtering, allowing the application to work on networks where it would otherwise be blocked or heavily throttled.

(Modified from Dispatcher's [README](https://github.com/OperatorFoundation/shapeshifter-dispatcher/blob/master/README.md) )