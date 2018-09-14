---
layout: single
author_profile: false
permalink: /implement/marionette-server/

title: "Marionette - Server"

header:
  overlay_image: /assets/images/unsplash_0wdpet-ufqs-todd-diemer.jpg
  caption: "Photo credit: [**CC0 Unsplash / Todd Diemer**](https://unsplash.com/@todd_diemer)"
#  cta_label: "More Info"
#  cta_url: "https://unsplash.com"

sidebar:
    nav: "sidenav"
---

At the end of August 2018, the [Marionette repository](https://www.github.com) received a substantial update, including detailed documentation that can help developers get started with Marionette, and implement it within their own software. The guides are simple to follow, but assume knowledge of the development environment. In this guide, we'll take you through the steps needed to get Marionette working on a server, using a CentOS server on DigitalOcean's cloud infrastructure. For installing the application on a Mac and connecting with Firefox, see our other [guide](/implement/marionette-client).

We've chosen CentOS here because it's what the Marionette developers have tested the compiling of Marionette with.

## Installing CentOS on Digital Ocean

First, you'll need to commission a server with DigitalOcean. At the time of writing, there are five different types of Linux, each with multiple versions. Choose CentOS, version 7.5 x64.

DigitalOcean publish their own guides to setting up the server. It is recommended that you set up a user with root privileges, and disable the root account. Instructions for this are available at [https://www.digitalocean.com/community/tutorials/initial-server-setup-with-centos-7](https://www.digitalocean.com/community/tutorials/initial-server-setup-with-centos-7). This basic guide covers these steps:

* Log in as root
* Create a user with root privileges \(sudo\)
* Enable key-based SSH authentication

The next Digital Ocean guide has some additional guides to setting up a firewall, adjusting your time settings and creating a swapfile. [https://www.digitalocean.com/community/tutorials/additional-recommended-steps-for-new-centos-7-servers](https://www.digitalocean.com/community/tutorials/additional-recommended-steps-for-new-centos-7-servers)

## Dependencies

Most of the dependencies for Marionette will be installed as part of the build process in the documentation. However, you will also need:

* Development Tools - including a C/C++ compiler, and shasum for verifying a downloaded file is genuine.
* Go - The open source language in which Marionette is written.

### Development Tools

This is a group of applications that don't come pre-installed on your CentOS server, but some of which will be needed for compiling Marionette and its dependencies. We're also going to install perl-Digest-SHA, which allows the calculation of SHA messages, and Nano, which is a text editor. To install the developer tools, simply run these two commands:

~~~~~
$ sudo yum group install -y "Development Tools"
$ sudo yum install -y perl-Digest-SHA
$ sudo yum install nano
~~~~~

### Golang

Go is a popular programming language that is used by Marionette, shapeshifter-dispatcher and other Pluggable Transports-related tools. To install, you will need to download and install it on your server. Here, we are downloading version 1.11, which is currently the latest version. See [https://golang.org/dl](https://golang.org/dl) to see if a newer version has been released.

~~~~~
$ curl -LO https://dl.google.com/go/go1.11.linux-amd64.tar.gz
$ sudo tar -C /usr/local -xzf go1.11.linux-amd64.tar.gz
~~~~~

This will install Go into the directory ```/usr/local/go```. Now you will set up some variables so that your server and Go know where to keep files, using the nano text editor:

~~~~~
$ nano ~/.bash_profile
~~~~~

You should add these lines to the end of the file:

~~~~~
export GOPATH=~/go
export GOBIN=$GOPATH/bin
export PATH=$PATH:/usr/local/go/bin:$GOPATH/bin
~~~~~

Type Ctrl-X to exit the editor, and answer Y to save the file. To make the new paths effective immediately, run the command:

~~~~~
source ~/.bash_profile
~~~~~

We set the value of ```GOPATH``` to ```~/go```. This means that Go will put its source code into ```~/go/src``` and compiled programs into ```~/go/bin```.


### Dep

Dep is a dependency manager for go. Just prior to installing Marionette, you will run Dep to make sure all of the dependencies are installed. To install Dep, you will first need to create a ```bin``` directory for Go, then fetch it from Github, and finally run the install script:

~~~~~
$ mkdir $GOPATH\bin
$ go get github.com/golang/dep
$ cd $GOPATH/src/github.com/golang/dep
$ ./install.sh
~~~~~

If you now type ```dep``` you should see that it is installed, and it will return a page of usage instructions.

## Installing Marionette

You now have most of the software you need to build Marionette. You will now download the source code using Go, and start the build as described in the (User Guide)[https://github.com/redjack/marionette/blob/master/doc/USER_GUIDE.md].

~~~~~
$ go get github.com/redjack/marionette
~~~~~

At this stage, you will get errors saying that libraries do not exist. These can be safely ignored, as they will be built in the next part of the process, which is building the third party scripts from the Marionette directory.

~~~~~
$ cd $GOPATH/src/github.com/redjack/marionette
$ ./build_third_party.sh
~~~~~

There will be **a lot** of information scrolling on the screen as the third party libraries are created. This process will take time, as the server will be fetching and compiling large files. You can check if it was successful with the command:

~~~~~
$ ls $GOPATH/src/github.com/redjack/marionette/third_party/libs
~~~~~

This should return a list of four files: ```libfst.a```, ```libre2.a```, ```libfstscript.a```, ```libgmp.a```

At this stage, you should now be ready to check dependencies and compile Marionette itself. However, you will need to fully install one of the third-party libraries:

~~~~~
$ cd $GOPATH/src/github.com/redjack/marionette/third_party/gmp-6.1.2
$ ./configure --enable-cxx
$ make
$ sudo make install
$ make check
~~~~~

One final check and download of dependencies:

~~~~~
$ cd $GOPATH/src/github.com/redjack/marionette
$ dep ensure
~~~~~

Now you will build Marionette with the following Go command:

~~~~~
$ go install ./cmd/marionette
~~~~~

Once that's complete, the Marionette binary is in ```$GOPATH/bin``` and you're ready to go! Remember, all of these steps only have to be completed once. From now on, if you need to restart your server, you can go straight to the command to run Marionette.

## Running Marionette

For our reference implementation, we're going to start the server to run as a SOCKS5 proxy:

~~~~~
$ marionette server -format http_simple_blocking -socks5
~~~~~

The response should be:

~~~~
listening on [::]:8081, proxying via socks5
~~~~

Next, you'll need to [configure a client](/implement/marionette-client) and connect your browser to your new Marionette server.