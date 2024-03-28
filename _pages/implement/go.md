---
layout: single
author_profile: false
permalink: /implement/go/

title: "Pluggable Transports in Your Go Application"

excerpt: "CC-BY Brandon Wiley"
header:
  overlay_image: /assets/images/unsplash_0wdpet-ufqs-todd-diemer.jpg
  caption: "Photo credit: [**CC0 Unsplash / Todd Diemer**](https://unsplash.com/@todd_diemer)"
#  cta_label: "More Info"
#  cta_url: "https://unsplash.com"

sidebar:
    nav: "impnav"
---
## How to Use Pluggable Transports in Your Go Application

{% include toc icon="file-text" %}

The following is an excerpt from 2018 by Dr. Brandon Wiley, The Operator Foundation

Transports that implement the PT 2.0 Go API (see the [PT2 spec](/spec/)) provide a “virtual network interface” that can be used instead of Go’s net.Conn API for sending and receiving traffic over the network. When using these transports, the application data is transformed so as to be resistant to blocking.

Before we get started writing code, let’s look at the required interfaces. These interfaces can be found in the PT 2.0 Go API specification, as well as in the [*shapeshifter-transports*](https://github.com/OperatorFoundation/shapeshifter-transports) Go library (in the [*base*](https://github.com/OperatorFoundation/shapeshifter-transports/blob/master/transports/base/base.go) package).

First, let’s look at the Transport interface. This is what a transport provides:

~~~~
type Transport interface {
	NetworkDialer() net.Dialer
	Dial(address string) TransportConn
	Listen(address string) TransportListener
}
~~~~

A Transport is anything that provides these methods.

- The **Dial** method is used to create an outgoing transport connection, using an address string, which contains the IP or domain name and the port for the destination transport server.

- The **Listen** method is used to listen for incoming transport connections, also using an address string for the IP and port on which to listen.

- The **TransportConn** and **TransportListener** types are also defined in the “base” module and will be covered next.

- The *NetworkDialer()* function returns the net.Dialer instance that the transport will use for making network connections. Accessing this object can be useful because it allows for setting options on the Dialer, such as setting timeout values.

The implementation of the Transport instance will implicitly also have an initializer function, which can take configuration information specific to the transport.

Next, we’ll look at the TransportListener interface. This is what a transport provides in order to be able to listen for transport connections:

~~~~
type TransportListener interface {
	NetworkListener() net.Listener
	TransportAccept() (TransportConn, error)
	Close() error
}
~~~~

Most of the work of implementing the TransportListener interface is in TransportAccept(), which listeners for a new transport connection and returns a TransportConn. Inside of TransportAccept(), the usual method of obtaining a transport connection is to listen for a standard network connection using an instance of the net.Listener interface. The NetworkListener() method reduces this net.Listener used by the TransportListener. This can be useful for setting network options. It is up to the TransportListener implementation to handle creating a net.Listener. Finally, the Close() method stops the TransportListener from accepting any more transport connections. Usually, the implementation of this method will also call Close() on the underlying net.Listener, stopping any incoming network connections.

## Example Transport-Enabled Application

To complete the discussion of the PT 2.0 Go API interface, we will provide an example implementation of the interface which implements the venerable UNIX “discard” network service, with all of the network traffic being sent using the obfs2 transport. The discard service listens for a TCP connection and then receives any data sent to it until the connection is closed. This is perhaps the simplest example of network programming and will allow us to see how transports can be used in place of the standard Go network library.

### Creating a Go Program

Before we get into using a transport, we must first do some common housekeeping to set up the code as a Go program.

#### 1. Define the Package

Each Go program must be defined in the main package:

`package main`

This line tells the Go compiler that this code is a Go program. The name of the program is determined by how you name the file.

#### 2. Import Dependencies

Before writing the Go code necessary to use the transport, required packages must be imported. Each transport is in its own package, so you only need to import the code for the transports that you use in your application. For this example application, the following dependencies are required:

~~~~
import (	
“bufio”
“fmt”
	“os”
	"github.com/OperatorFoundation/shapeshifter-transports/transports/base"
	"github.com/OperatorFoundation/shapeshifter-transports/transports/obfs2"
)
~~~~

The “fmt” package is a standard Go package that is only used here for debugging. The “base” package is required to get the interface types, such as Transport and TransportListener. The “obfs2” package contains the specific transport that we are using in this example.

#### 3. Create the Main Function

Each Go program must have a main function which contains the code that is run initially when the program is started. We can create an empty main function and then add code to it as we continue with the example. The empty main function is defined as follows:

~~~~
func main() {
	// Application example code goes here
}
~~~~

#### 4. Name the Files

Go code intended to be compiled as a program must be named according to the intended name of the program. The file must also be in a directory with the same name. We will be implementing both a client and a server, so let’s save our code so far into two files, “pt-discard-server/pt-discard-server.go” and “pt-discard-client/pt-discard-client.go”.

### Using a Transport In a Server Application

#### 1. Initialize the Transport

We get a new instance of the Transport interface by calling the initializer function for the chosen transport. For obfs2, the initializer can be calling with the following:

`var transport base.Transport = obfs2.NewObfs2Transport()`

This creates a new variable “transport” that provides an obfs2 transport implementing the Transport interface.

#### 2. Obtain a Transport Connection Listener

In order to accept new transport connection, we need a transport connection listener. This is obtained from the transport instance by calling Listen() with the address on which to listen, as follows:

`var listener base.TransportListener = transport.Listen(“0.0.0.0:1234”)`

This creates a new variable “listener” that provides an obfs2 transport connection listener implementing the TransportListener interface on the special IP address “0.0.0.0” and on the port 1234. The IP address “0.0.0.0” signifies any valid address on the local machine.

Alternatively, a specific IP address could be used. However, this requires knowing the IP address of the machine on which the server is running. For this simple example, we will just use “0.0.0.0” to listen on all IPs on that machine. The port 1234 is just an example port. There is no standard port for running transports. Since transports are designed to circumvent Internet filtering, using a standard port would be counterproductive as that port would be blocked by the filters.

**IMPORTANT: Do not use the port 1234 for your transport server when using a transport in a real application. Pick your own port for your application, or use a randomly selected port.**

#### 3. Accept Transport Connections

The next step in an application server using transports is to accept incoming transport connections. This is accomplished by calling TransportAccept() on the TransportListener interface. It is standard to call this in a loop so that multiple connections will be accepted up until the server application is terminated. If it were not called in a loop, the program would exit after the first transport connection closed. The listen loop is written as follows:

~~~~
for {
	var conn base.TransportConn
	var acceptErr error
	conn, acceptErr = listener.TransportAccept()
	if acceptErr != nil {
		return
	}
	// Additional code goes here
}
~~~~

This loop runs forever (until the program is terminated) and accepts transport connections. If an error is encountered accepting a connection, then the program exits. The rest of the code for this example will also go inside this loop.

#### 4. Receive Data Over Transport Connection

Once the transport connection has been accepted, the server can read and write data over that connection. In the case of our discard server example, the server should read data and then discard it until the connection is closed, as follows:

~~~~
var buffer []byte = make([]byte, 1024)
bytesRead, err := conn.Read(buffer)
for {
if err != nil {
		return
	}

	fmt.Println("Received", bytesRead)
	bytesRead, err = conn.Read(buffer)
}
~~~~

This creates a new variable “buffer” and allocates an array of 1024 bytes which is assigned to this new variable and then reads from the transport connection into this buffer. This read also creates new variables “bytesRead” and “err”. The “bytesRead” variable is used to track the number of bytes read each time Read() is called. In the example app, this is just used for debugging purposes. The “err” variable is used to record errors encountered while calling Read(). In particular, if the connection is closed then Read() will return an error, otherwise it will return nil. The code then executes a loop. Each time through the loop it checks the “err” variable returned by Read(). If Read() returned an error instead of nil, then the loop exits. Next, we print the number of bytes read, simply for debugging purposes. Finally, Read() is called again, overwriting the “bytesRead” and “err” variables for the next loop iteration.

This loop structure is common in Go code. It is the same loop you would use to write networking code using net.Conn. Since the TransportConn interface extends the net.Conn interface, using transports is similar to standard networking code.

### Using a Transport In a Client Application

#### 1. Initialize the Transport

We get a new instance of the Transport interface by calling the initializer function for the chosen transport. For obfs2, the initializer can be calling with the following:

`var transport base.Transport = obfs2.NewObfs2Transport()`

This creates a new variable “transport” that provides an obfs2 transport implementing the Transport interface.

#### 2. Obtain a Transport Connection

In order to create a new transport connection, we need to call the Dial() method on the transport instance, as follows:

`var conn base.TransportConn = transport.Dial(“127.0.0.1:1234”)`

This creates a new variable “conn” that provides an obfs2 transport connection implementing the TransportConn interface. The transport connection is made to the special IP address “127.0.0.1” and on the port 1234. The IP address “127.0.0.1” signifies the address of the local machine. This assumes that, for demonstration and testing purposes, you are running the transport server and client on the same machine. In actual use, a specific IP address would be used. However, this requires knowing the IP address of the machine on which the server is running. For this simple example, we will just use “127.0.0.1” to connect to a transport server on the same machine as the client. The port 1234 is just an example port. There is no standard port for running transports. Since transports are designed to circumvent Internet filtering, using a standard port would be counterproductive as that port would be blocked by the filters.

**IMPORTANT: Do not use the port 1234 for your transport server when using a transport in a real application. Pick your own port for your application, or use a randomly selected port.**

#### 3. Obtain Application Data

The next step in an application client is to obtain the application’s data that needs to be sent over the transport. How this is achieved depends on the specific application. For our simple example discard client, we will read the data from the client program’s standard input. In order to do this easily, we can make a bufio.Reader for standard input and then use it to read the data one line at a time, as follows:

~~~~
reader := bufio.NewReader(os.Stdin)
for {
	text, _ := reader.ReadString('\n')

	// Additional code goes here
}
~~~~

This code creates a new bufio.Reader for the standard input data stream. It then loops forever, using the bufio.Reader to read the input data one line at a time. The data goes into the new variable “text”.

#### 4. Send Data Over Transport Connection

Once the transport connection has been accepted, the server and read and write data over that connection. In the case of our discard server example, the server should read data and then discard it until the connection is closed, as follows:

~~~~
var buffer []byte = make([]byte, 1024)
for bytesRead, err := conn.Read(buffer); err != nil; bytesRead, err = conn.Read(buffer) {
fmt.Println("Received %d bytes", bytesRead)
}
~~~~

This creates a new variable “buffer” and allocates an array of 1024 bytes which is assigned to this new variable. It then executes a loop. The first time through the loop, it creates new variables “bytesRead” and “err”. The “bytesRead” variable is used to track the number of bytes read each time Read() is called. In the example app, this is just used for debugging purposes. The “err” variable is used to record errors encountered while calling Read(). In particular, if the connection is closed then Read() will return an error, otherwise it will return nil. The loop continues as long as “err” is nil. While the loop is executing, it will continue to call Read(). Inside the loop, we print the number of bytes read, simply for debugging purposes.

---

## PT API 3.0: How to Use Pluggable Transports in Your Go Application

The most recent version of the Pluggable Transports specification, version 3.0 as of 2023, counts with an available sample implementation for the Go language, which can be found [here](https://github.com/Pluggable-Transports/Pluggable-Transports-spec/blob/main/releases/PTSpecV3.0/Pluggable%20Transport%20Specification%20v3.0%20-%20Go%20Transport%20API%20v3.0.md)


*CC-BY Dr. Brandon Wiley, The Operator Foundation*
