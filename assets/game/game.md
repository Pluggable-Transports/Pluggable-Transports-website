Many interchangeable transports are available, following a single specification.
They hide traffic, making it indistinguishable from routine / allowed traffic.
An app using PTs can switch between transports to restore online access...
... without the need for user intervention.

------------
Play through our guide to Pluggable Transports [here](http://linktotheguide)

------------
Who are you?
I'm a developer
I'm an end user

- Go back
- I'm done!
- Start over
-------------
Pluggable Transports for Developers
You already know you want to use Pluggable Transports with your software, and are looking for some additional resources. What can we help you with today?
- I want to install support for Pluggable Transports on my OpenVPN server
- Using Pluggable Transports in my Go app
- Testing Pluggable Transports
- I want to write a transport
- Pluggable Transports Specification
-------------
OpenVPN and Pluggable Transports
If you're here, you've probably got an OpenVPN server running, and you want to add support for Pluggable Transports. However, maybe you need some help installing OpenVPN?
- Yes, please help me with OpenVPN
- I have OpenVPN running, let's talk Pluggable Transports
-------------
Installing and Configuring OpenVPN
Our [guide to installing OpenVPN](https://www.pluggabletransports.info/implement/openvpn/#preparation) takes you through these three steps:

Preparation: Getting the dependencies (openssl, ca-certificates, git, golang, curl, screen).
OpenVPN installation: How to install, configure and test OpenVPN with root / sudo access on your server.
Install and configure certificates: Setting up the OpenVPN network, and generating certificates for the server and clients.

Once that's complete, and you're happy with the basic setup, it's time to move on to the Pluggable Transports configuration.

- Move on to Pluggable Transports
-------------
Installing Pluggable Transports on OpenVPN
You have an OpenVPN server running, and have tested client connections. You now want to add Pluggable Transports support. To do this, you're going to install (shapeshifter-dispatcher)[https://github.com/OperatorFoundation/shapeshifter-dispatcher], which will allow you to proxy both TCP and UDP traffic.

Our guide to installing Pluggable Transports with OpenVPN will help you [install and configure shapeshifter](https://www.pluggabletransports.info/implement/openvpn/#server-obfuscation-configuration).

The steps you will be guided through are:

* Install and configure shapeshifter-dispatcher
* Server and client setup

Once this is done, you will have your own server running OpenVPN with Pluggable Transports!

Alternatively, you could run [this script](https://github.com/OpenInternet/openvpn-shapeshifter) to install and configure OpenVPN on Ubuntu and Debian servers.

Are you done with this guide?

-------------
Using Pluggable Transports in Go
Our full guide for using Pluggable Transports in Go is available [here](https://www.pluggabletransports.info/implement/go/). We take you through these steps in a sample deployment:

* What are the Transport and TransportListener interfaces?
* Creating a Go program
* Using a Transport in a Server application
* Using a Transport in a Client application

Once you're done with that guide, you might want to look at how to test your app in different environments.

------------
Testing Pluggable Transports
There are many different ways in which access to sites and services can be blocked, ranging from simple IP address or DNS blocking to rules created from deep packet inspection \(DPI\). You might want to test a network environment that simulates some of the many ways in which this can happen.

We recommend investigating the following services:

[Adversary Lab](https://github.com/OperatorFoundation/AdversaryLab): Created by Operator Foundation, with support from the Pluggable Transports community. Adversary Lab is a service that analyzes captured network traffic to extract statistical properties. Using this analysis, filtering rules can be synthesized to block sampled traffic.

[CoPilot](https://openinternet.github.io/copilot/): This is a wireless hotspot that provides an easy to use web interface for simulating custom censorship environments. It comes with various plugins and the ability to import censorship “rules” from standard Intrusion prevention and detection systems.

------------

Thanks for following our story! Let us know what you think of this - you can [email us](mailto:contact@pluggabletransports.info) or find us on [Twitter](https://twitter.com/plugtransports). For other useful links, visit the [Pluggable Transports web site](https://www.pluggabletransports.info/), and see these [community links](https://www.pluggabletransports.info/community) for more info.

------------

Writing Pluggable Transports

Do you have an idea for a Pluggable Transport? Do you want to implement something that follows the current spec and can help improve the community's work?

The first place to start is to look at the [Pluggable Transports specification](locallink). There is also a [Transports Library](https://www.pluggabletransports.info/transports/) that will tell you about the existing transports.

When you're ready to move on, our [online guide](https://www.pluggabletransports.info/build/how/) will talk you through creating a transport in the Go programming language, by providing an example implementation of the interface which provides a simple ROT13 cipher on the contents of the application data stream.

Don't forget to also look at [the Internews small grants pool](https://www.surveymonkey.com/r/pluggabletransports) to investigate funding for new Pluggable Transports.

------------

The current version of the spec is available [here](https://github.com/Pluggable-Transports/Pluggable-Transports-spec/tree/master/releases/PTSpecV2.1Draft1). 

The developments that led to the Pluggable Transports 2.0 specification included:

* An API for the Go programming language
* Support for the UDP protocol
* A wider variety of apps and implementations for different environments

Version 2.1 reframed this as a modular specification, so that components of the spec can be upgraded independently, and developers can be compliant even if their work is tied to one platform.

As you would expect, the specification for Pluggable Transports continues to evolve. You may find that the current spec doesn't work for your needs, and you might want to contribute to the next version. If that's the case, you can do that via [the Github repo](https://github.com/Pluggable-Transports/Pluggable-Transports-spec/tree/master/releases/PTSpecV2.1Draft1).
