---
title: "PTIM2020 Guest Blog - Spec Update"
modified: 2020-10-11T13:20:02-05:00
categories:
  - blog
tags:
  - transport
  - PTIM2020
  - Conference
  - Virtual
  - Specification
layout: posts

---

Our latest blog is from Dr. Brandon Wiley of Operator Foundation. As a major contributor to the emerging v2.2 specification, he takes us through some of the key changes that you can expect to see published.


## What’s Coming in the Pluggable Transports Specification v2.2?

### Dr. Brandon Wiley, [Operator Foundation](https://operatorfoundation.org)

The following proposals are under consideration for incorporation into the v2.2 draft of the Pluggable Transports Specification:

&nbsp;&nbsp;&nbsp;&nbsp; 1. Improved Logging in IPC <br />
&nbsp;&nbsp;&nbsp;&nbsp; 2. Reduce Number of Required IPC Parameters <br />
&nbsp;&nbsp;&nbsp;&nbsp; 3. User Settable Proxy Listen Address <br />
&nbsp;&nbsp;&nbsp;&nbsp; 4. Reduce Use of Microformats in IPC Parameters <br />




A brief summary of each of the proposals follows:

### 1. Improved Logging in IPC
This proposal adds a mechanism for the dispatcher to convey logging information to the host application. This is accomplished through adding a new IPC protocol message to carry logging information. There is also a new command line option, <br />```-ipcLogLevel```, which allows users to control what level of logging is passed to the host application.

### 2. Reduce Number of Required IPC Parameters
This proposal simplifies usage of the dispatcher by making as many parameters as possible optional. Sensible defaults are provided for parameters that were previously required.

### 3. User Settable Proxy Listen Address
This proposal allows users of the dispatcher to specify the address and port on which to listen when running in client mode. Previously, there was no way to specify this information. When in client mode, the dispatcher would pick a random port. This command line option has been one of the most requested features.

### 4. Reduce Use of Microformats in IPC Parameters
This proposal provides new command line options for specifying addresses and ports. These new options are designed to reduce confusion from users of the dispatcher as this has been a common support issue. Instead of specifying the transport, host, and port all in one parameter which must be formatted correctly, these are now separate parameters. This simplifies usage as users do not need to know a special format for this information.

### Conclusion
All of the changes for the PT v2.2 specification are backwards-compatible with PT v2.1. These are minor changes which are designed to make the dispatcher easier to use. There are no changes to the Go API or Swift API parts of the specification, only to the IPC protocol used by the dispatcher. Use of these new features is optional and all PT v2.1 configurations will still work with the new PT v2.2 dispatcher. All of these proposals have been fully implemented. The version of the dispatcher including all of these proposed features is available here: [https://github.com/OperatorFoundation/shapeshifter-dispatcher/tree/dev](https://github.com/OperatorFoundation/shapeshifter-dispatcher/tree/dev)