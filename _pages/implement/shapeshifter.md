---
layout: single
author_profile: false
permalink: /implement/shapeshifter/

title: "Installing shapeshifter-dispatcher"

header:
  overlay_image: /assets/images/unsplash_0wdpet-ufqs-todd-diemer.jpg
  caption: "Photo credit: [**CC0 Unsplash / Todd Diemer**](https://unsplash.com/@todd_diemer)"
#  cta_label: "More Info"
#  cta_url: "https://unsplash.com"

sidebar:
    nav: "impnav"
---

{% include toc icon="file-text" %}

As covered in our [How to Write a Pluggable Transport](/build/how) guide, there are two ways to use transports for all the versions of the specification so far, including version 3.0: the Transports API (which defines a set of language-specific APIs to use transports directly from within an application) and the Dispatcher. This guide covers the Dispatcher, which is a command line tool that runs in a separate process and is controlled through a custom inter-process communication (IPC) protocol whose interface provides a way to integrate with applications written in any language and to wrap existing applications in PTs without needing to modify the source code.

As mentioned in Operator Foundation’s README of April 2023, if you are a tool developer working in the Go programming language, then you will probably want to use the [transports library](https://github.com/OperatorFoundation/shapeshifter-transports) directly in your application.

## Supported modes and transports

As of version 3.0.1 of the shapeshifter-dispatcher, the dispatcher currently supports the following proxy modes:

- SOCKS5 (with optional PT 2.0 authentication protocol)
- Transparent TCP
- Transparent UDP
- STUN UDP

The transports used by shapeshifter-dispatcher follow the Go Transport API in the Pluggable Transports Specification v3.0 and currently the ones currently supported are the following:

- Replicant
- Optimizer
- shadow (Shadowsocks)
Note: obs4 is no longer supported. We recommend using Shadow in its place.


This guide assumes you already have a server installed with Go version 1.17 or higher, as well as initialized the Go module for this project. If you need help, please follow our [Basic Server Setup](/implement/basicserver#installing-go) guide. The instructions below are adapted from [https://github.com/OperatorFoundation/shapeshifter-dispatcher](https://github.com/OperatorFoundation/shapeshifter-dispatcher). They will take you through downloading shapeshifter and proving that it works at its most basic level. For using shapeshifter-dispatcher with OpenVPN, see [our guide here](/implement/openvpn).

If you already have Go installed, make sure it is a compatible version:
~~~~~
go version
~~~~~


# Downloading and building shapeshifter-dispatcher #

Get the git repository for shapeshifter-disptacher:

~~~~
git clone https://github.com/OperatorFoundation/shapeshifter-dispatcher.git
~~~~

Go into that directory and build the command line executable:

~~~~
cd shapeshifter-dispatcher
go install
~~~~
This will fetch the source code for shapeshifter-dispatcher, and all the dependencies, compile everything, and put the result in /bin/shapeshifter-dispatcher.


# Running using Netcat #

We're going to use Netcat, a network tool to communicate between machines. Traffic is received on your server by shapeshifter, and passed to the port that Netcat is listening to. On the client, Netcat sends its traffic to shapeshifter to obfuscate and send to the server. This means the path for data sent to the server and back is:

Client: Netcat -> Client: shapeshifter ---> Server: shapeshifter -> Server: Netcat -> Server: shapeshifter ---> Client: shapeshifter -> Client: Netcat

As of version 3.0.1 the argument usage is as follows:
- `orport` has been replaced with `target`, as noted in this [issue](https://github.com/Pluggable-Transports/Pluggable-Transports-website/issues/187)
- Use either `-client` or `-server` to place the proxy into client or server mode, respectively
 - The default proxy mode is SOCKS5 (with optional PT 2.1 authentication protocol), which only supports proxy SOCKS5-aware TCP connections
 - Use the `-transparent` flag for Transparent TCP proxy mode
 - Use the `-udp` flag to enable UDP proxying for STUN packet proxying and protocols such as WebRTC, which are based on top of STUN
 - Using the `-transparent` flag along with the `-udp` flag enables Transparent UDP proxy mode
 - Only one proxy mode can be used at a time
- Use `-state` to specify a directory to put transports state information
- Use `-transports` to specify which transports to launch
- Use `-optionsFile` to specify the directory where your config file is located

### Running using Netcat and the Replicant transport
To use Replicant, a config file is needed. A sample config file, located in ConfigFiles/ReplicantServerConfigV3.json, is provided purely for educational purposes and should not be used in actual production.
To use Replicant on a production server, you will need a key pair from Operator Foundation, which you can request by mailing [contact@operatorfoundation.org](mailto:contact@operatorfoundation.org).

For this example to work, you need an application server running, in this guide we’ll use netcat to run a simple server on port 3333:
~~~~~
nc -l 3333
~~~~~
This will start a server session, listening on port 3333.

#### Server
To launch the transport server, telling it where to find the application server, run:

~~~~
<GOPATH>/bin/shapeshifter-dispatcher -transparent -server -state state -target 127.0.0.1:3333 -transports Replicant -bindaddr Replicant-127.0.0.1:2222 -optionsFile ConfigFiles/ReplicantServerConfigV3.json -logLevel DEBUG -enableLogging
~~~~
This runs the server in transparent TCP proxy mode. The directory "state" is used to hold transport state. The destination that the server will proxy to is 127.0.0.1, port 3333. The Replicant transport is enabled and bound to the address 127.0.0.1 and the port 2222. Logging is enabled and set to DEBUG level. To access the Log for debugging purposes, look at state/dispatcher.log.

#### Client
~~~~
<GOPATH>/bin/shapeshifter-dispatcher -transparent -client -state state -transports Replicant -proxylistenaddr 127.0.0.1:1443 -optionsFile ConfigFiles/ReplicantClientConfigV3.json -logLevel DEBUG -enableLogging
~~~~
This runs the client in transparent TCP proxy mode. The directory "state" is used to hold transport state. The address of the server is specified as 127.0.0.1, port 2222. This is the same address as was specified on the server command line above. For these commands to work, the dispatcher server needs to be running on this host and port. The Replicant transport is enabled and bound to the address 127.0.0.1 and the port 1443.

Once the client is running, you can connect to the client address, which in this case is 127.0.0.1, port 1443. For instance, you can telnet to this address:
~~~~
telnet 127.0.0.1 1443
~~~~
Any bytes sent over this connection will now be forwarded through the transport server to the application server, which in the case of this example is the netcat server.

Lastly, and as mentioned above, for using Replicant in this example a sample config file (located in ConfigFiles/ReplicantServerConfigV3.json) is employed but it should not be used in actual production.

An additional guide using SOCKS5 Mode is available [here](https://github.com/OperatorFoundation/shapeshifter-dispatcher#running-in-socks5-mode)

### Config generator
To generate a new pair of configs for any of the supported transports, run the following command:
~~~~
<GOPATH>/bin/shapeshifter-dispatcher -generateConfig -transport <transport name> -serverIP <serverIP:Port>
~~~~

For Replicant, you can also add the flags -toneburst and/or -polish if you would like to enable the Starburst toneburst and the Darkstar polish respectively.

## Credits
shapeshifter-dispatcher is descended from the Tor project's "obfs4proxy" tool.

- David Fifield for goptlib
- Adam Langley for the Go Elligator implementation
- Philipp Winter for the ScrambleSuit protocol
- Shadowsocks was developed by the Shadowsocks team
