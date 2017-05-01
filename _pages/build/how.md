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
    nav: "sidenav"
---

How to Write a Pluggable Transport
==================================

This document provides a run-through of creating a Pluggable Transport, using a [ROT13 Examples transport](https://github.com/OperatorFoundation/shapeshifter-transports/blob/master/examples/rot13/rot13.go) from the [Shapeshifter Transports](https://github.com/OperatorFoundation/shapeshifter-transports) repository. It is worth noting that the Shapeshifter implementation of [ShadowSocks](https://github.com/OperatorFoundation/shapeshifter-transports/tree/master/transports/shadow) wraps the shadowsocks-go library directly, without any modification of the shadowsocks-go code, provides an example of leveraging an external Go library. This external library approach helps ensure that it will remain up-to-date whenever the upstream library is updated.

<!-- {% include toc title="" icon="file-text" %} -->

**CC-BY Brandon Wiley, The Operator Foundation** 

Pluggable Transports can be written in any programming language. A Pluggable Transport interacts with with a host application using a type of inter-process communication (IPC) protocol which is described in the Pluggable Transports 2.0 specification. There are Pluggable Transport implementations written in Go, Python, C++, and C. Some developers choose to implement their transports from scratch or using a library that implements some parts of the IPC protocol, such as shapeshifter-ipc or goptlib. A faster alternative than developing from scratch is to implement the transport in Go, using the Go API provided in the Pluggable Transports 2.0 specification. This method of implementing a transport is currently limited to transports implemented in the Go programming language. However, there are some advantages to this approach. Applications that are also written in Go can use transports implementing the PT 2.0 Go API directly, bypassing the IPC layer. This decreases the complexity of integration, as well as the performance overhead of running the transports in a separate process. Additionally, the Operator Foundation provides a tool called Shapeshifter Dispatcher, which wraps transports implementing the PT 2.0 Go API to provide the IPC layer. This allows applications written in programming languages other than Go to use these transports with no additional development cost.

Transport Connections vs Network Connections
--------------------------------------------

An important distinction to remember when implementing a transport is the difference between transport connections and network connections. A transport connection is a communication channel between a transport client and a transport server, which is capable of communication using the chosen transport protocol. A network connection is a communication channel between two computers, which communicates data streams that can be in any protocol. For convenience, the API for making transport connections mimics closely the interface for making network connections. A transport connection essentially looks to the application like a “virtual network connection”. However, the actual mapping between transport connections and network connections depends on the transport. Some transports have exactly one network connection for each transport connection. Other transports may split the transport connection’s data over multiple network connections, or multiplex multiple transport connections over one network connection. A transport could even have 0 network connections, instead conveying data using a connectionless alternative such as UDP.

While the Pluggable Transport API is flexible enough to accommodate a variety of mappings between transport connections and network connections, it also provides support for the common case of one transport connection for each network connection. In particular, the underlying network connection for a transport connection, if there is one, can be retrieved and accessed directly. This is useful for doing lower-level configuration of the network, such as setting network options on the network connection.

How to Write a Pluggable Transport in Go
========================================

Implementing a Pluggable Transport in Go is simply a matter of implementing the methods defined in the PT 2.0 Go API specification. Due to the way that Go handles interface types, it is not even necessary to import any dependencies to build a transport. If the transport implements the correct methods, then it will work with applications that are designed to use transports.

Before we get started writing code, let’s look at the required interfaces. These interfaces can be found in the PT 2.0 Go API specification, as well as in the [*shapeshifter-transports*](https://github.com/OperatorFoundation/shapeshifter-transports) Go library (in the [*base*](https://github.com/OperatorFoundation/shapeshifter-transports/blob/master/transports/base/base.go) package).

First, let’s look at the Transport interface. This is what a transport needs to provide:

~~~~
type Transport interface {
	NetworkDialer() net.Dialer
	Dial(address string) TransportConn
	Listen(address string) TransportListener
}
~~~~

A Transport is anything that provides these methods. The Dial method is used to create an outgoing transport connection, using an address string, which contains the IP or domain name and the port for the destination transport server. The Listen method is used to listen for incoming transport connections, also using an address string for the IP and port on which to listen. The TransportConn and TransportListener types are also defined in the “base” module and will be covered next. The NetworkDialer() function returns the net.Dialer instance that the transport will use for making network connections. Accessing this object can be useful because it allows for setting options on the Dialer, such as setting timeout values.

All that is required for a transport is that it can make outgoing connections, listen for incoming connections, and set network options (by accessing the net.Dialer instance). The details of what the transport actually does will be provided by the TransportConn and TransportListener implementations, which will be covered next. Before we move on from the Transport interface, however, it should also be noted that the implementation of the Transport instance will implicitly also have an initializer function, which can take configuration information specific to your transport.

Next, we’ll look at the TransportListener interface. This is what a transport needs to provide in order to be able to listen for transport connections:

~~~~
type TransportListener interface {
	NetworkListener() net.Listener
	TransportAccept() (TransportConn, error)
	Close() error
}
~~~~

Most of the work of implementing the TransportListener interface is in TransportAccept(), which listeners for a new transport connection and returns a TransportConn. Inside of TransportAccept(), the usual method of obtaining a transport connection is to listen for a standard network connection using an instance of the net.Listener interface. The NetworkListener() method reduces this net.Listener used by the TransportListener. This can be useful for setting network options. It is up to the TransportListener implementation to handle creating a net.Listener. Finally, the Close() method stops the TransportListener from accepting any more transport connections. Usually, the implementation of this method will also call Close() on the underlying net.Listener, stopping any incoming network connections.

Example Transports
==================

To complete the discussion of the PT 2.0 Go API, we will provide an example implementation of the interface which provides a simple ROT13 cipher on the contents of the application data stream. This code can be found in the shapeshifter-transports [*examples/rot13*](https://github.com/OperatorFoundation/shapeshifter-transports/blob/master/examples/rot13/rot13.go) module.


Creating a Go Module
--------------------

Before we get into implementing a transport, we must first do some common housekeeping to set up the code as a Go module.

### Define the Package

Each Go module must be defined in a package:

~~~~
package rot13
~~~~

This line tells the Go compiler that this module will be called rot13. In Go, you import the module using the full path to the file. In the case of this package, which is part of shapeshifter-transports, you would import it as “github.com/OperatorFoundation/shapeshifter-transports/examples/rot13". Within code that uses the module, methods will be referred to with the short name “rot13”.

### Import Dependencies

Before writing the Go code necessary to implement the transport, required packages must be imported. This will be different for every transport. For this example transport, the following dependencies are required:

~~~~
import (
	"fmt"
	"net"
	"time"
	"golang.org/x/net/proxy"
	"github.com/OperatorFoundation/shapeshifter-ipc"
	"github.com/OperatorFoundation/shapeshifter-transports/transports/base"
)
~~~~

Implementing Transport
----------------------

The goal in implementing a transport is to implement all of the required methods for the Transport interface. However, in Go these methods need to be attached to a data structure. So the first step is to define a data structure using Go’s “struct” feature.

### Create the Struct

Technically, the struct could be empty. However, it is a common practice among transports to put any state needed by the transport into the struct. In particular, the Transport interface requires the NetworkDialer() method to return a net.Dialer. We can accomplish this easily by storing a net.Dialer in our transport’s struct.

The struct for our ROT13 example transport is defined as follows:

~~~~
type rot13Transport struct {
	dialer \*net.Dialer
}

### Create the Initializer

The last bit of housekeeping we need to do before we get started with the transport implementation is to create an initializer function. In order to call the Transport methods on our struct, we need an instance of this struct to be created. In Go, you can create a struct with a struct literal. However, it can be a nicer interface to provide an initializer function. The standard way to create a transport in Go is to make the struct private and then to provide a public initializer function. This ensures that the only way new Transport instances are created is through calling the initializer function.

The initializer function for our ROT13 example transport is defined as follows:

~~~~
func NewRot13Transport() \*rot13Transport {
	return &rot13Transport{dialer: nil}
}
~~~~

As this is a simple example, the initializer function takes no arguments and simple wraps the standard Go struct literal syntax for allocating a new instance of our data structure using a default nil value for the dialer property. However, please note that this initializer is a normal function. In a more complex example, it could take arguments and do any number of transformations on the arguments, providing default values for fields, and calling other helper functions. It is much more flexible than a struct literal, which is why we prefer to use initializer functions instead.

Now that we have a struct on which to attach methods, we must implement the methods required by the Transport interface.

### NetworkDialer

The NetworkDialer method is straightforward in this example, as we have already stored a net.Dialer instance on our struct. The NetworkDialer() implementation is defined as follows:

~~~~
func (transport \*rot13Transport) NetworkDialer() net.Dialer {
	return \*transport.dialer
}

This implementation simply returns the net.Dialer instance stored in our struct. Of course, this function could do something more complex instead, such as creating a new net.Dialer if one does not already exist. In the case of transports that do not use network connections at all (such as transports that use UDP for communication), this function could simply return nil.

### Dial

The Dial function is responsible for making new transport connections. In our simple example, each transport connection is mapped to one network connection. Therefore, this method must also establish a network connection each time it is called.

The Dial function is defined as follows:

~~~~
func (transport *rot13Transport) Dial(address string) base.TransportConn {
	dialFn := proxy.Direct.Dial
	conn, dialErr := dialFn("tcp", address)
	if dialErr != nil {
		return nil
	}
	dialConn := conn
	transportConn, err := newRot13ClientConn(conn)
	if err != nil {
		dialConn.Close()
		return nil
	}
}
~~~~

This function is divided into two parts. The first part makes the network connection. The second part wraps the network connection in our transport implementation using the newRot13ClientConn() initializer function that is defined further along in the example.

### Listen

The Listen() is responsible for accepting new incoming transport connections. In our simple example, each transport connection is mapped to one network connection. Therefore, this method must also listen for incoming network connections.

The Listen function is defined as follows:

~~~~
func (transport *rot13Transport) Listen(address string) base.TransportListener {
	addr, resolveErr := pt.ResolveAddr(address)
	if resolveErr != nil {
		fmt.Println(resolveErr.Error())
		return nil
	 }

	ln, err := net.ListenTCP("tcp", addr)
	if err != nil {
		fmt.Println(err.Error())
		return nil
	}
	return newRot13TransportListener(ln)
}
~~~~

This function is divided into three parts. The first part resolves the supplied address string to toe type of address needed by the net.ListenTCP() fucnction. The second part listens for an incoming network connection. The third part wraps the network connection in our transport implementation using the newRot13TransportListener() initializer function that is defined further along in the example.

Implementing TransportListener
------------------------------

The next step in creating a transport is to implement the TransportListener interface. This is what is returned from the Listen() method on the Transport interface, and internally in our example implementation from the newRot13TransportListener() function. The TransportListener is responsible for accepting new incoming transport connections. The interface we need to implement is defined as follows:

~~~~
type TransportListener interface {
	NetworkListener() net.Listener
	TransportAccept() (TransportConn, error)
	Close() error
}
~~~~

Before we implement these methods, we need to create a data structure on which to attach them, and an initializer function to create instances of this data structure.

### Create the Struct

Since our transport will use one network connection for each transport connection, we will need a net.TCPListener to listen for the incoming network connections. We can store this as a field in our struct so that we can reuse it.

We define the internal struct type rot13TransportListener as follows:

~~~~
type rot13TransportListener struct {
	listener \*net.TCPListener
}
~~~~

### Create the Initializer

The initializer function for our ROT13 example transport listener is defined as follows:

~~~~
func newRot13TransportListener(listener \*net.TCPListener) \*rot13TransportListener {
	return &rot13TransportListener{listener: listener}
}
~~~~

As this is a simple example, the initializer function simply wraps the given net.TCPListener.

### NetworkListener

The NetworkListener method is straightforward in this example, as we have already stored a net.Dialer instance on our struct. The NetworkDialer() implementation is defined as follows:

~~~~
func (listener \*rot13TransportListener) NetworkListener() net.Listener {
	return listener.listener
}

This implementation simply returns the net.TCPListener instance stored in our struct. In the case of transports that do not use network connections at all (such as transports that use UDP for communication), this function could simply return nil.

### Close

The Close() function is straightforward. To end incoming transport connections, we just call Close on the network listener stored in the transport listener struct, as follows:

~~~~
func (listener \*rot13TransportListener) Close() error {
	return listener.listener.Close()
}
~~~~

### TransportAccept

The TransportAccept function is responsible for waiting for new incoming transport connections. In our simple example, each transport connection is mapped to one network connection. Therefore, this method must also wait for a new incoming network connection each time it is called.

The TransportAccept function is defined as follows:

~~~~
func (listener \*rot13TransportListener) TransportAccept() (base.TransportConn, error) {
	conn, err := listener.listener.Accept()
	if err != nil {
		return nil, err
	}

return newRot13ServerConn(conn)

}
~~~~

This function is divided into two parts. The first part waits for a new incoming network connection. The second part wraps the network connection in our transport implementation using the newRot13ServerConn() initializer function that is defined further along in the example.

Implementing TransportConn
--------------------------

The core of most transports is the TransportConn implementation. The purpose of the other interfaces (Transport, TransportListener) is to get to the point that a TransportConn is available. The interesting part, which makes the transport unique from other transports, is usually in the TransportConn, specifically in the Read() and Write() methods. This is discussed in great detail further on in the example.

The interface we need to implement is defined as follows:

~~~~
type TransportConn interface {
	net.Conn
	NetworkConn() net.Conn
}
~~~~

The net.Conn interface, which we also need to implements, is defined as follows:

~~~~
type Conn interface {
	Read(b \[\]byte) (n int, err error)
	Write(b \[\]byte) (n int, err error)
	Close() error
	LocalAddr() Addr
	RemoteAddr() Addr
	SetDeadline(t time.Time) error
	SetReadDeadline(t time.Time) error
	SetWriteDeadline(t time.Time) error
}
~~~~

What this syntax means in Go is that the TransportConn interface extends the net.Conn interface. Every method that exists on net.Conn also exists on TransportConn. In Go, net.Conn represents a network connection. My implementing all of the same methods as a net.Conn, any implementation of TransportConn implements a “virtual network connection”. It can be used as a drop-in replacement for a normal network connection, making it easy to integrate into existing applications written in Go that use networking. So implementions of TransportConn must implement all of the net.Conn methods and, additionally, we add one new method, NetworkConn(), which returns a normal net.Conn. The purpose of this method is to allow access to the actual network connection being used for the transport connection. This is useful for doing things such as setting network options on the connection. One way to look at this is that a TransportConn wraps a net.Conn and the NetworkConn() method allows it to be unwrapped to access the internal net.Conn.

### Create the Struct

Since our transport will use one network connection for each transport connection, we will need a net.Conn to represent the actual network connection. We can store this as a field in our struct so that we can reuse it.

We define the internal struct type rot13Conn as follows:

~~~~
type rot13Conn struct {
	conn net.Conn
}
~~~~

### Create the Initializers

We have two initializer functions for our ROT13 example transport connections, which are defined as follows:

~~~~
func newRot13ClientConn(conn net.Conn) (c \*rot13Conn, err error) {
	// Initialize a client connection, and start the handshake timeout.
	return &rot13Conn{conn: conn}, nil
}

func newRot13ServerConn(conn net.Conn) (c \*rot13Conn, err error) {
	// Initialize a server connection, and start the handshake timeout.
	return &rot13Conn{conn}, nil
}
~~~~

As this is a simple example, the initializer functions simply wraps the given net.Conn. There are two initializers, one for the client and one for the server, because it is common for transports to have different initialization steps depending on whether the transport is on the client or server side. However, since our example is simple, these functions end up being identical.

### NetworkConn

The NetworkConn method is straightforward in this example, as we have already stored a net.Conn instance on our struct. The NetworkConn() implementation is defined as follows:

~~~~
func (transportConn \*rot13Conn) NetworkConn() net.Conn {
	return transportConn.conn
}
~~~~

This implementation simply returns the net.Conn instance stored in our struct. In the case of transports that do not use network connections at all (such as transports that use UDP for communication), this function could simply return nil.

### net.Conn

In addition to the NetworkConn() method that we defined in the TransportConn interface, all of the methods inherited from net.Conn must also be implemented.

#### Write(b \[\]byte) (n int, err error)

The Write() method (along with the Read() method) is the core of most transports. This is where the transformation of the application protocol data stream into the transport’s protocol is made, the reason for the existence of transports. The application which is using the Pluggable Transport uses the TransportConn as if it were net.Conn, substituting a “virtual network connection” for a real one. It will repeatedly call Write() to write its application data. The Write() method is therefore given the application data as an argument, transforms it, and then writes the transformed data to the network connection.

The ROT13 example of Write() is defined as follows:

~~~~
func (conn \*rot13Conn) Write(b \[\]byte) (int, error) {
	shift(b)
	i, err := conn.conn.Write(b)
	if err != nil {
		return 0, err
	}
	return i, err
}
~~~~

In the first part of the code, the data is passed to the shift() function, which implements the ROT13 cipher, mutating the given data. The transformed data is then written to the network connection.

#### Read(b \[\]byte) (n int, err error)

The Read() method (along with the Write() method) is the core of most transports. This is where the transport’s protocol is transformed back into the application protocol data stream. The application which is using the Pluggable Transport uses the TransportConn as if it were net.Conn, substituting a “virtual network connection” for a real one. It will repeatedly call Read() to get its application data from the network. The Read() method therefore reads the already transformed data from the network connection, reverse the transformation to retrieve the application data, and returns it.

The ROT13 example of Read() is defined as follows:

~~~~
func (conn \*rot13Conn) Read(b \[\]byte) (int, error) {
	i, err := conn.conn.Read(b)
	if err != nil {
		return 0, err
	}
	unshift(b)
	return i, err
}
~~~~

In the first part of the code, the already transformed data is read from the network connection. In the second part of the code, the already transformed data is passed to the unshift() function, which implements the ROT13 cipher, mutating the given data. The transformation that happened in Write() is reversed and the recovered data is returned.

#### Other net.Conn Methods

For the rest of the net.Conn methods, since this is a simple example that uses one network connection for each transport connection, the TransportConn just calls the corresponding method on the net.Conn contained in the rot13Conn struct. These methods are defined as follows:

~~~~
func (conn \*rot13Conn) LocalAddr() net.Addr {
	return conn.conn.LocalAddr()
}

func (conn \*rot13Conn) RemoteAddr() net.Addr {
	return conn.conn.RemoteAddr()
}

func (conn \*rot13Conn) SetDeadline(t time.Time) error {
	return conn.conn.SetDeadline(t)
}

func (conn \*rot13Conn) SetReadDeadline(t time.Time) error {
	return conn.conn.SetReadDeadline(t)
}

func (conn \*rot13Conn) SetWriteDeadline(t time.Time) error {
	return conn.conn.SetWriteDeadline(t)
}
~~~~

### Implementing the ROT13 Cipher

The TransportConn implementation uses the shift() and unshift() methods to transform the application data to and from the transport protocol. The transport protocol is a simple one, with the only change being that the data is encrypted before being sent over the network using a ROT13 cipher. The implementation of the ROT13 cipher is as follows:

~~~~
func shift(b \[\]byte) {
	for i := 0; i &lt; len(b); i++ {
		if b\[i\] &gt; (255 - 13) {
			b\[i\] = b\[i\] - (255 - 13) + 13
		} else {
			b\[i\] += 13
		}
	}

}

func unshift(b \[\]byte) {
	for i := 0; i &lt; len(b); i++ {
		if b\[i\] &lt; 13 {
			b\[i\] = b\[i\] + (255 - 13) - 13
		} else {
			b\[i\] -= 13
		}
	}
}

~~~~

