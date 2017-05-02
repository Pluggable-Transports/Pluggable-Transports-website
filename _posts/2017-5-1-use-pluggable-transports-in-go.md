---
title: "How to Use Pluggable Transports in Your Application"
modified: 2017-05-01T12:20:02-05:00
categories:
  - blog
tags:
  - implement
  - IPC
  - Go
  - API
  - developer
layout: archive

---

Also by [Operator Foundation](https://operatorfoundation.org)'s Brandon Wiley is a new guide for application developers interested in implementing pluggable transports using the Go API.

Applications that are also written in Go can use transports implementing the PT 2.0 Go API directly, bypassing the IPC layer. This decreases the complexity of integration, as well as the performance overhead of running the transports in a separate process. Additionally, the Operator Foundation provides a tool called Shapeshifter Dispatcher, which wraps transports implementing the PT 2.0 Go API to provide the IPC layer. This allows applications written in programming languages other than Go to use these transports with no additional development cost.

Read [How to Use Pluggables Transports in Your Application](/implement/go) for a complete walkthrough for adding pluggable transports.

