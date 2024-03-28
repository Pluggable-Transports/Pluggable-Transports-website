---
layout: single
title: "How to Write a Pluggable Transport"
permalink: /build/how/

excerpt: ""
header:
  overlay_image: /assets/images/unsplash_Francisco Casero_empty-track.jpg
  caption: "Photo credit: [**CC0 Unsplash / Francisco Casero**](https://unsplash.com)"
#  cta_label: "More Info"
#  cta_url: "https://unsplash.com"

sidebar:
    nav: "createnav"
---

How to Write a Pluggable Transport
==================================


{% include toc title="" icon="file-text" %}

This document provides a run-through of creating a Pluggable Transport following instructions developed by the Operator Foundation as part of their extensive work and research on Pluggable Transports.

Using the Pluggable Transports 2.1 specification
=================================================

Pluggable Transports can be written in any programming language. A Pluggable Transport interacts with with a host application using a type of inter-process communication (IPC) protocol which is described in the Pluggable Transports specification (see versions [here](https://github.com/Pluggable-Transports/Pluggable-Transports-spec/tree/main/releases)). Currently, there are Pluggable Transport implementations available in Go, Python, C++, and C, with the open source documentation for PT libraries implementing the PT API version 3.0 in Go, Swift, and Java [here](https://github.com/Pluggable-Transports/Pluggable-Transports-spec/tree/main/releases/PTSpecV3.0). Some developers choose to implement their transports from scratch or using a library that implements some parts of the IPC protocol, such as shapeshifter-ipc or goptlib. A faster alternative than developing from scratch is to implement the transport in Go, using the Go API provided in the Pluggable Transports 2.0 specification. This method of implementing a transport is currently limited to transports implemented in the Go programming language. However, there are some advantages to this approach. Applications that are also written in Go can use transports implementing the PT 2.0 Go API directly, bypassing the IPC layer. This decreases the complexity of integration, as well as the performance overhead of running the transports in a separate process. Additionally, the Operator Foundation provides a tool called Shapeshifter Dispatcher, which wraps transports implementing the PT 2.0 Go API to provide the IPC layer. This allows applications written in programming languages other than Go to use these transports with no additional development cost.
There are two ways to use transports for all the versions of the specification so far, including version 3.0. The Transports API defines a set of language-specific APIs to use transports directly from within an application. Alternatively, transports can be used through the Dispatcher, a command line tool that runs in a separate process and is controlled through a custom inter-process communication (IPC) protocol whose interface provides a way to integrate with applications written in any language and to wrap existing applications in PTs without needing to modify the source code.

Transport Connections vs Network Connections
--------------------------------------------

An important distinction to remember when implementing a transport is the difference between transport connections and network connections. A transport connection is a communication channel between a transport client and a transport server, which is capable of communication using the chosen transport protocol. A network connection is a communication channel between two computers, which communicates data streams that can be in any protocol. For convenience, the API for making transport connections mimics closely the interface for making network connections. A transport connection essentially looks to the application like a “virtual network connection”. However, the actual mapping between transport connections and network connections depends on the transport. Some transports have exactly one network connection for each transport connection. Other transports may split the transport connection’s data over multiple network connections, or multiplex multiple transport connections over one network connection. A transport could even have 0 network connections, instead conveying data using a connectionless alternative such as UDP.

Particularly for the Swift language, it is important to differentiate between the Network framework, which provides an API for making TCP and UDP network connections and the Network Extension framework, which is for managing VPNs. In the case of Pluggable Transports, both of these frameworks come into play as Swift Pluggable Transports may be running inside of a Network Extension, but use the Network framework to connect to the transport server.
While the Pluggable Transport API is flexible enough to accommodate a variety of mappings between transport connections and network connections, it also provides support for the common case of one transport connection for each network connection. In particular, the underlying network connection for a transport connection, if there is one, can be retrieved and accessed directly. This is useful for doing lower-level configuration of the network, such as setting network options on the network connection.


PT 2.0 Draft 3: How to Write a Pluggable Transport in Go
========================================================

Before we get started writing code, let’s look at the required interfaces. For [PT 2.0](https://github.com/Pluggable-Transports/Pluggable-Transports-spec/tree/main/releases/PTSpecV2.0) these interfaces can be found in the  Go API specification on section 3.2.4

First, let’s look at the Transport interface. This is what a transport needs to provide:

~~~~
type Transport interface {
    // Note that there is no place in this interface to provide the 
    // transport configuration since it’s provided in the initializer function

    // Create outgoing transport connection
    // The Dial method implements the Client Factory abstract interface
     Dial(address string) net.Conn

     // Create listener for incoming transport connection
     // The Listen method implements the Server Factory abstract interface
     Listen(address string) net.Listener

}
~~~~

A Transport is anything that provides these methods. The Dial method is used to create an outgoing transport connection, using an address string, which contains the IP or domain name and the port for the destination transport server. The Listen method is used to listen for incoming transport connections, also using an address string for the IP and port on which to listen. The TransportConn and TransportListener(now net.Conn and net.Listener since PT 2.0 Draft 3) types are also defined in the “base” module and will be covered next. The NetworkDialer() function returns the net.Dialer instance that the transport will use for making network connections. Accessing this object can be useful because it allows for setting options on the Dialer, such as setting timeout values.

All that is required for a transport is that it can make outgoing connections, listen for incoming connections, and set network options (by accessing the net.Dialer instance). The details of what the transport actually does will be (net.Conn and net.Listener, respectively), which will be covered next. Before we move on from the Transport interface, however, it should also be noted that the implementation of the Transport instance will implicitly also have an initializer function, which can take configuration information specific to your transport.

Next, we’ll look at the Listener interface. This is what a transport needs to provide in order to be able to listen for transport connections:

~~~~
type Listener interface {
	// Accept waits for and returns the next connection to the listener
	Accept() (net.Conn, error)

	// Close closes the listener
	Close() error
	// Addr returns the listener's network address
	Addr() Addr
} 
~~~~

Last, we’ll look at the Conn interface. 
~~~~
// net.Conn implements the Connection abstract interface
type Conn interface {
	// The transport-specific logic for obfuscating network traffic is
	// implemented inside the methods defined in the net.Conn interface

	// Read reads data from the transport connection, will likely also require 
	// reading data from the underlying network connection and is where the
	// transport-specific logic for de-obfuscating network traffic is implemented
	Read(b []byte) (n int, err error)

	// Write writes data to the connection which may or may not result in
	// immediate writing of data to the underlying network connection.
	// The transport-specific logic for obfuscating network traffic is
	// implemented here
	Write(b []byte) (n int, err error)

	// Close closes the transport connection and the underlying network
	// connection used by the transport.
	Close() error

	// These methods are also part of the net.Conn interface
	// For more information on these methods, look at the official
	// net.Conn documentation.
	LocalAddr() Addr
	RemoteAddr() Addr
	SetDeadline(t time.Time) error
	SetReadDeadline(t time.Time) error
	SetWriteDeadline(t time.Time) error
}


~~~~


To complete the discussion of the PT 2.0 Go API to implement a transport, a constructor function must be created that returns an instance of the Transport interface. The transport constructor function, being a normal Go function, can take arbitrary configuration parameters. It is up to the application using the API to implement a valid call to the constructor function for the specific transport being used.

As shown above, the Transport instance has two main pieces of functionality: dialing outgoing connections and listening for incoming connections. An instance of the Transport interface must implement a Dial method for making outgoing connections and a Listen method for handling incoming connections.

Listening for incoming connections is handled by an instance of the net.Listener interface. An Accept() function allows for accepting a new incoming transport connection and the Close() function stops listening for incoming connections.

The transport will also need to implement instances of the net.Conn interface. The Dial and Listen functions both return instances of the net.Conn interface. In most cases, these will be different implementations of the interface, one for encoding traffic into the transport’s specific protocol and the other for decoding this traffic.

Overall, all network operations are delegated to the transport. For instance, the transport is responsible for initiating outgoing network connections and listening for incoming network connections. This gives the transport flexibility in how it uses the network.

Please refer to the [PT 2.0](https://github.com/Pluggable-Transports/Pluggable-Transports-spec/tree/main/releases/PTSpecV2.0) specification document for further details and considerations.



PT 3.0: How to Write a Pluggable Transport in Swift
===================================================

To implement the Transport API for the Swift programming language, the application makes calls to library functions to configure the transports and sends and receives data using functions that provide a socket-like interface. This interface is most useful if you are seeking to build a transport directly into your application which is also written in the Swift programming language. 

The Swift API for transports provides a set of interfaces and implementations that mimic those found in the Network framework. Modifying code from using the Network framework directly to using transports is a simple matter of renaming the types. For instance, the NWConnection class has been extended to conform to the Connection interface, and instead of using the NWConnection class, one can use the Connection interface. Therefore, the type Connection can be used everywhere in the code, whether the code is using Pluggable Transports or directly connecting to the network using the Network framework.

Goals for interface design have been introduced in section 3.2.1 of the base version of the specification as follows:
* Transport implementers have to do the minimum amount of work in addition to implementing the core transform logic.
* Transport users have to do the minimum amount of work to add PT support to code that uses standard networking primitives from the language or platform.
* Transports may or may not generate, send, receive, store, and/or update persistent or ephemeral state.
    * Transports that do not need persistence or negotiation can interact with the application through the simplest possible interface
    * Transports that do need persistence or negotiation can rely on the application to provide it through the specified interface, so the transport does not need to implement persistence or negotiation internally.
* Applications should be able to use a PT Client implementation to establish several independent transport connections with different parameters, with a minimum of complexity and latency.
* The interface in each language should be idiomatic and performant, including reproducing blocking behavior and interaction with nonblocking IO subsystems when possible.

Definition of the ClientFactory interface that transports must provide:
~~~~
protocol ConnectionFactory
{
	connect(using: NWParameters) -> Connection?
}
~~~~

Usage example:
~~~~
class MeekConnectionFactory: ConnectionFactory
{
	init(to: URL, serverURL: URL)
	init(to: URL, serverURL: URL, factory: ConnectionFactory)
	connect(using: NWParameters) -> Connection?
}
~~~~

Here the first init() is a basic initializer for the Meek transport. It takes transport-specific arguments, in this case the "to" and “serverURL” parameters. The connect() method can then be called multiple times to create connections. The connect method takes an NWParameters argument to specify information related to the Network framework. For instance, this parameter is how you determine whether the returned connection is a TCP or UDP socket.

The second init() allows for the Meek transport to take an existing ConnectionFactory to use to make its connection to the network. This allows for nesting of Meek with other transports. If no factory is provided, Meek will use the standard NWConnection provided by the Network framework to connect to the network.

Additional examples for logging, using interfaces for TCP connections and UDP sessions, and more can be found in the [Swift Transport API](https://github.com/Pluggable-Transports/Pluggable-Transports-spec/blob/main/releases/PTSpecV3.0/Pluggable%20Transport%20Specification%20v3.0%20-%20Swift%20Transport%20API%20v3.0.md) reference implementation of PT 3.0.


 
Next, take a look at some examples of software already using Pluggable Transports [here](/implement).



---

*CC-BY Dr. Brandon Wiley, The Operator Foundation*
