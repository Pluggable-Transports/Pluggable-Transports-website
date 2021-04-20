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

This table shows some examples of software already using Pluggable Transports across the most popular platforms: 

| --------------||
| **Desktop** | [Tor Browser](https://torproject.org), [Lantern](https://getlantern.org), [Psiphon](https://psiphon.ca) |
| **Mobile - Android** | [Briar Messaging](https://briarproject.org), [Lantern](https://getlantern.org), [Orbot](https://guardianproject.info/apps/orbot/), [Psiphon](https://psiphon.ca), [Calyx VPN](https://calyx.net) |
| **Mobile - iOS** | [OnionBrowser](https://itunes.apple.com/us/app/onion-browser-secure-anonymous-web-with-tor/id519296448?mt=8), [Psiphon](https://itunes.apple.com/bm/app/psiphon/id1276263909), [Psiphon Browser](https://itunes.apple.com/ca/app/psiphon-browser/id1193362444?mt=8) |

{% include toc icon="file-text" %}

If you're a software developer looking to deploy Pluggable Transports, the rest of the content on this plage will help you. We will look at some of the options for implementation, focusing mainly on Go - the language already used by many PT developers implementers.

## Implementation Options

Developers can implement transports from scratch or using a library that implements some parts of the IPC protocol, such as shapeshifter-ipc or goptlib. Pluggable Transports themselves can be written in any programming language. A Pluggable Transport interacts with with a host application using a type of inter-process communication (IPC) protocol which is described in the [Pluggable Transports 2.1 specification](/spec/). There are Pluggable Transport implementations written in Go, Python, C++, and C.

If you just want to add existing transports into your application, this table summarizes the libraries that have already been developed and are available for us:

| --------------||
| **Desktop** | [PT 2.1 Go API](/implement/go/) |
| **Mobile - Android** | [PLUTO 2](https://github.com/guardianproject/AndroidPluggableTransports), [NetCipher](https://github.com/guardianproject/NetCipher)
| **Mobile - iOS** | [Swift API](https://github.com/Pluggable-Transports/Pluggable-Transports-spec/blob/master/releases/PTSpecV2.1Draft1/Pluggable%20Transport%20Specification%20v2.1%20-%20Swift%20Transport%20API%20v1.0%2C%20Draft%201.pdf) |


Implementation outside of these environments can be done with [Shapeshifter Dispatcher](https://github.com/OperatorFoundation/shapeshifter-dispatcher), a command-line tool which provides a proxy that wraps the transport library. It has several different proxy modes and can proxy both TCP and UDP traffic.

The purpose of the dispatcher is to provide different proxy interfaces to using transports. Through the use of these proxies, application traffic can be sent over the network in a form that bypasses network filtering, allowing the application to work on networks where it would otherwise be blocked or heavily throttled.

## Transport Connections vs Network Connections

An important distinction to remember when implementing a transport is the difference between transport connections and network connections. A transport connection is a communication channel between a transport client and a transport server, which is capable of communication using the chosen transport protocol. A network connection is a communication channel between two computers, which communicates data streams that can be in any protocol. For convenience, the API for making transport connections mimics closely the interface for making network connections. A transport connection essentially looks to the application like a “virtual network connection”. However, the actual mapping between transport connections and network connections depends on the transport. Some transports have exactly one network connection for each transport connection. Other transports may split the transport connection’s data over multiple network connections, or multiplex multiple transport connections over one network connection. A transport could even have 0 network connections, instead conveying data using a connectionless alternative such as UDP.

While the Pluggable Transport API is flexible enough to accommodate a variety of mappings between transport connections and network connections, it also provides support for the common case of one transport connection for each network connection. In particular, the underlying network connection for a transport connection, if there is one, can be retrieved and accessed directly. This is useful for doing lower-level configuration of the network, such as setting network options on the network connection.

---

*This page contains content from Dispatcher's [README](https://github.com/OperatorFoundation/shapeshifter-dispatcher/blob/master/README.md) and documentation by Dr. Brandon Wiley*



