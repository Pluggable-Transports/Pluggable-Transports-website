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

This table represents the current options for current applications using pluggable transports and options for deploying transports in new applications across the most popular platforms.

| | **Windows** | **OSX** | **Linux Server** | **Android** | **iOS** |
|----|----|----|----|-------|---|
| **Applications using PTs**    | [Tor Browser](http://torproject.org/), [Lantern](https://getlantern.org/), [Psiphon 3](https://www.psiphon3.com/)                   | [Tor Browser](http://torproject.org/), [Lantern](https://getlantern.org/),           | [Tor](https://www.torproject.org/docs/tor-doc-unix.html.en), [psiphon-tunnel-core](https://github.com/Psiphon-Labs/psiphon-tunnel-core) | [Orbot](https://guardianproject.info/apps/orbot/), [Lantern](https://play.google.com/store/apps/details?id=org.getlantern.lantern&hl=en), [Psiphon 3](https://s3.amazonaws.com/0ubz-2q11-gi9y/en.html), [FreeBrowser](https://freebrowser.org/) | [OnionBrowser](https://itunes.apple.com/us/app/onion-browser-secure-anonymous-web-with-tor/id519296448?mt=8)                        |
| **PT Services**               | [Dispatcher](https://github.com/OperatorFoundation/shapeshifter-dispatcher), [obfs4proxy](https://github.com/Yawning/obfs4),        | [Dispatcher](https://github.com/OperatorFoundation/shapeshifter-dispatcher), [obfs4proxy](https://github.com/Yawning/obfs4),        | [Dispatcher](https://github.com/OperatorFoundation/shapeshifter-dispatcher), [obfs4proxy](https://github.com/Yawning/obfs4),            | [Dispatcher](https://github.com/OperatorFoundation/shapeshifter-dispatcher), [obfs4proxy](https://github.com/Yawning/obfs4),                      | [OnionBrowser custom obfs4proxy](https://github.com/mtigas/iObfs)                    |
| **Integration Library**       | PT 2.0 Go API                         | PT 2.0 Go API                         | PT 2.0 Go API                             | [PLUTO](https://github.com/guardianproject/pluto), [NetCipher](https://github.com/guardianproject/NetCipher)                                      | [OnionBrowser custom API](https://github.com/mtigas/OnionBrowser)                    |
| **Implementation Transports** | [shapeshifter-ipc (Go)](https://github.com/OperatorFoundation/shapeshifter-ipc), [goptlib (Go)](https://github.com/Yawning/goptlib) | [shapeshifter-ipc (Go)](https://github.com/OperatorFoundation/shapeshifter-ipc), [goptlib (Go)](https://github.com/Yawning/goptlib) | [shapeshifter-ipc (Go)](https://github.com/OperatorFoundation/shapeshifter-ipc), [goptlib (Go)](https://github.com/Yawning/goptlib)     | [shapeshifter-ipc (Go)](https://github.com/OperatorFoundation/shapeshifter-ipc), [goptlib (Go)](https://github.com/Yawning/goptlib)               | [shapeshifter-ipc (Go)](https://github.com/OperatorFoundation/shapeshifter-ipc), [goptlib (Go)](https://github.com/Yawning/goptlib) |

{% include toc icon="file-text" %}

## Implementation Options

Developers can implement transports from scratch or using a library that implements some parts of the IPC protocol, such as shapeshifter-ipc or goptlib. Pluggable Transports themselves can be written in any programming language. A Pluggable Transport interacts with with a host application using a type of inter-process communication (IPC) protocol which is described in the Pluggable Transports 2.0 specification. There are Pluggable Transport implementations written in Go, Python, C++, and C. 

A faster alternative than developing from scratch is to implement the transport in Go, using the [Go API](/implement/go) provided in the Pluggable Transports 2.0 specification. This method of implementing a transport is currently limited to transports implemented in the Go programming language. However, there are some advantages to this approach. Applications that are also written in Go can use transports implementing the PT 2.0 Go API directly, bypassing the IPC layer. This decreases the complexity of integration, as well as the performance overhead of running the transports in a separate process. Additionally, the Operator Foundation provides a tool called Shapeshifter Dispatcher, which wraps transports implementing the PT 2.0 Go API to provide the IPC layer. This allows applications written in programming languages other than Go to use these transports with no additional development cost.

<!--
***Future***

|                               | **Windows**   | **OSX**       | **Linux**     | **Android**                | **iOS**                          |
|-------------------------------|---------------|---------------|---------------|----------------------------|----------------------------------|
| **PT Services**               | Dispatcher    | Dispatcher    | Dispatcher    | Dispatcher                 | Dispatcher, PT Network Extension |
| **Integration Library**       | PT 2.1 Go API | PT 2.1 Go API | PT 2.1 Go API | PT 2.1 Java API (wraps Go) | PT 2.1 Swift API (native)        |
| **Implementation Transports** | goptlib2 (Go) | goptlib2 (Go) | goptlib2 (Go) | goptlib2 (Go)              | N/A                              |
-->

## What is Dispatcher?

There are two components: transports and the dispatcher. Each transport provide different approach to obfuscation. These transports are provided as a Go library which can be integrated directly into applications. The dispatcher is a command line tool which provides a proxy that wraps the transport library. It has several different proxy modes and can proxy both TCP and UDP traffic.

If you are a tool developer working in the Go programming language, then you probably want to use the [transports library](https://github.com/OperatorFoundation/shapeshifter-transports)  directly in your application. 

If you want a end user that is trying to circumvent filtering on your network or you are a developer that wants to add pluggable transports to an existing tool that is not written in the Go programming language, then you probably want the dispatcher. Please note that familiarity with executing programs on the command line is necessary to use this tool. 

The purpose of the dispatcher is to provide different proxy interfaces to using transports. Through the use of these proxies, application traffic can be sent over the network in a form that bypasses network filtering, allowing the application to work on networks where it would otherwise be blocked or heavily throttled.

## Transport Connections vs Network Connections

An important distinction to remember when implementing a transport is the difference between transport connections and network connections. A transport connection is a communication channel between a transport client and a transport server, which is capable of communication using the chosen transport protocol. A network connection is a communication channel between two computers, which communicates data streams that can be in any protocol. For convenience, the API for making transport connections mimics closely the interface for making network connections. A transport connection essentially looks to the application like a “virtual network connection”. However, the actual mapping between transport connections and network connections depends on the transport. Some transports have exactly one network connection for each transport connection. Other transports may split the transport connection’s data over multiple network connections, or multiplex multiple transport connections over one network connection. A transport could even have 0 network connections, instead conveying data using a connectionless alternative such as UDP.

While the Pluggable Transport API is flexible enough to accommodate a variety of mappings between transport connections and network connections, it also provides support for the common case of one transport connection for each network connection. In particular, the underlying network connection for a transport connection, if there is one, can be retrieved and accessed directly. This is useful for doing lower-level configuration of the network, such as setting network options on the network connection.

---

*Modified from Dispatcher's [README](https://github.com/OperatorFoundation/shapeshifter-dispatcher/blob/master/README.md) and documentation by Dr. Brandon Wiley*

<!-- 

# Mobile

* The Guardian Project's [Orbot](https://guardianproject.info/apps/orbot/) enables censorship circumvention for Android phones

* The Guardian Project's [PLUTO Library](https://github.com/guardianproject/pluto) specifically implements pluggable transports.

# Infrastructure

* [The Tor Project](https://trac.torproject.org/projects/tor/wiki/doc/PluggableTransports) has a page with a list of current transports, sample libraries, and upcoming concepts.

* The Tor Project has a large&nbsp;<a href="https://www.torproject.org/docs/pluggable-transports.html.en">documentation repository</a>

-->

