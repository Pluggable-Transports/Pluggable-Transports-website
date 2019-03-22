---
layout: single
author_profile: false
permalink: /implement/getready/

title: "Getting ready"

header:
  overlay_image: /assets/images/unsplash_0wdpet-ufqs-todd-diemer.jpg
  caption: "Photo credit: [**CC0 Unsplash / Todd Diemer**](https://unsplash.com/@todd_diemer)"
#  cta_label: "More Info"
#  cta_url: "https://unsplash.com"

sidebar:
    nav: "impnav"
---
## Getting ready for a Pluggable Transports project

{% include toc icon="file-text" %}

As you start to build your own Pluggable Transports projects, you're going to need some supporting tools. Most tools have step-by-step guides for implementation, but some assume that you will already have these essentials installed. You will most likely be building a client / server configuration - this means you will have a machine acting as a server (receiving data from a censored location and forwarding it to the open internet), and a machine acting as a client (from with a censored location, talking to your server).

Here are some of the ways to configure the environment you're using:

### Server
This could be on your local machine, or installed in a data center somewhere. Our instructions for [OpenVPN](/implement/openvpn) assume that you will be using an Ubuntu server (version 16.04.1 LTS). If you're looking to install Ubuntu in the cloud, consider using [DigitalOcean](https://www.digitalocean.com), who offer a range of pre-configured server setups depending on your needs. They also have their own [basic configuration guide](https://www.digitalocean.com/community/tutorials/initial-server-setup-with-ubuntu-16-04) for setting up the server and [setting up a firewall](https://www.digitalocean.com/community/tutorials/ufw-essentials-common-firewall-rules-and-commands). 

Digital Ocean also offer [CentOS](https://www.digitalocean.com/community/tutorial_series/new-centos-7-server-checklist), which is the server platform used for building the [Marionette](https://github.com/redjack/marionette/blob/master/doc/USER_GUIDE.md) binary.

### Client
Our list of [Transports](/implement) will tell you which ones work on which platform. If you're going to compile the software yourself, you will most likely need a Linux-style environment. The [marionette](https://github.com/redjack/marionette) project fully tested their build process on both Mac OSX and CentOS. In some cases, such as a mobile deployment, the software will be built on a desktop computer before being transferred to a mobile device.

### Go
The Go toolchain is the most commonly used language for Pluggable Transports. To install Go, you will need to follow the instructions at [https://golang.org/dl/](https://golang.org/dl/).

To get started with a Go program, you will make sure you have a GOPATH variable set, for example: 

~~~~~
export GOPATH=~/goprojects
~~~~~

You then download the go package from its URL

~~~~~
go get https://path.to.project/package
~~~~~

This will download the package you are going to install (into $GOPATH\src), and all of the dependencies that the developer has linked to in their code. When you are ready to build and install, follow the instructions from the developer. These instructions will usually include the command

~~~~~
go install package
~~~~~

This will compile the package and its dependencies, with the resulting binary located in the directory $GOPATH\bin

We also have a more in-depth look at Go, which teachees developers how to [implement transports in their own applications](/implement/go).

### Other libraries
Most other libraries will be retrieved and installed as part of an installation script. Marionette's instructions include running a file called build_third_party.sh before building the Go package. This is a shell script that downloads and compiles third-party binary files needed for Marionette (openfst, re2, and gmp-6.1.2). Always check the developer's instructions to make sure you have any dependencies installed before you perform the build.
