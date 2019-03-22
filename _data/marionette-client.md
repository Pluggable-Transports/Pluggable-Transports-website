---
layout: single
author_profile: false
permalink: /implement/marionette-client/

title: "Marionette - Client"

header:
  overlay_image: /assets/images/unsplash_0wdpet-ufqs-todd-diemer.jpg
  caption: "Photo credit: [**CC0 Unsplash / Todd Diemer**](https://unsplash.com/@todd_diemer)"
#  cta_label: "More Info"
#  cta_url: "https://unsplash.com"

sidebar:
    nav: "sidenav"
---

You've built Marionette [on your server](/implement/marionette-server), and now need to connect to it with your client. You're going to need to build the Marionette binary again, on the client this time, and configure it to connect to your server. These instructions are for the Mac OS, which has been tested against the [User Guide](https://github.com/redjack/marionette/blob/master/doc/USER_GUIDE.md) provided by the development team. 

## Dependencies

The starting point for this guide is that you have Mac OSX installed, and have admin priveliges on the machine as a user. Start a Terminal window from Applications/Utilities, or type ```terminal``` into Spotlight. This will launch a terminal window, into which you can type Linux commands.

### Golang

Go is a popular programming language that is used by Marionette, shapeshifter-dispatcher and other Pluggable Transports-related tools. To install, you will need to download and install it on your server. Here, we are downloading version 1.11, which is currently the latest version. See [https://golang.org/dl](https://golang.org/dl) to see if a newer version has been released.

~~~~~
$ curl -LO https://dl.google.com/go/go1.11.darwin-amd64.tar.gz
$ sudo tar -C /usr/local -xzf go1.11.darwin-amd64.tar.gz
~~~~~

This will install Go into the directory ```/usr/local/go```. Now you will set up some variables so that your server and Go know where to keep files, using the nano text editor:

~~~~~
$ nano ~/.profile
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

This should return a list of three files: ```libfst.a```, ```libre2.a```, ```libfstscript.a```

At this stage, you should now be ready to check dependencies and compile Marionette itself. However, you will need to fully install one of the third-party libraries:

~~~~~
$ cd $GOPATH/src/github.com/redjack/marionette/third_party/gmp-6.1.2
$ make install
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