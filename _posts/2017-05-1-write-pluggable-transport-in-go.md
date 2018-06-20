---
title: "How to Write a Pluggable Transport"
modified: 2017-05-01T13:20:02-05:00
categories:
  - blog
tags:
  - transport
  - library
  - Go
layout: posts

---

[Operator Foundation](https://operatorfoundation.org)'s Brandon Wiley has contributed a great 101 guide for building Pluggable Transports, with a deep-dive into writing transports in Go:

> Pluggable Transports can be written in any programming language. A Pluggable Transport interacts with with a host application using a type of inter-process communication (IPC) protocol which is described in the Pluggable Transports 2.0 specification. There are Pluggable Transport implementations written in Go, Python, C++, and C. Some developers choose to implement their transports from scratch or using a library that implements some parts of the IPC protocol, such as shapeshifter-ipc or goptlib. A faster alternative than developing from scratch is to implement the transport in Go, using the Go API provided in the Pluggable Transports 2.0 specification. This method of implementing a transport is currently limited to transports implemented in the Go programming language. However, there are some advantages to this approach. Applications that are also written in Go can use transports implementing the PT 2.0 Go API directly, bypassing the IPC layer. This decreases the complexity of integration, as well as the performance overhead of running the transports in a separate process. Additionally, the Operator Foundation provides a tool called Shapeshifter Dispatcher, which wraps transports implementing the PT 2.0 Go API to provide the IPC layer. This allows applications written in programming languages other than Go to use these transports with no additional development cost.

Read more at [How to Write a Pluggable Transport](/build/how/)
