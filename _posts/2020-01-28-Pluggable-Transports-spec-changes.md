---
title: "Pluggable Transports Specification Changes"
modified: 2020-01-28T13:20:02-05:00
categories:
  - blog
tags:
  - transport
  - library
  - Go
  - Swift
  - Operator
  - Spec
  - Specification
layout: posts

---
The current version of the Pluggable Transports specification is [here](https://github.com/Pluggable-Transports/Pluggable-Transports-spec), and is at version 2.1

We are now looking for community input for revising the specification. There are already som 




[Operator Foundation](https://operatorfoundation.org) recently completed and released two new prototypes for its Optimizer transport, for use with Go and Swift.

The Go prototype can be accessed from Operator's main Go transport repository at [https://github.com/OperatorFoundation/shapeshifter-transports](https://github.com/OperatorFoundation/shapeshifter-transports), while the Swift implementation can be accessed at [https://github.com/OperatorFoundation/Shapeshifter-Swift-Transports](https://github.com/OperatorFoundation/Shapeshifter-Swift-Transports). The two implementations have feature parity with regards to strategy modules, although the Swift version makes use of additional strategies designed for the Apple-specific environment.

In addition to this work, Operator have added Optimizer support to [shapeshifter-dispatcher](https://github.com/OperatorFoundation/shapeshifter-dispatcher), which allows Optimizer to be used in other environments, through the PT 2.1 Inter-process communication \(IPC\) specification. This extra development has also involved a modification to shapeshifter-dispatcher to allow options to be read from a configuration file rather than specified on the command line.

Full documentation for shapeshifter-dispatcher and the Optmizer transports are contained in the repositories' README files.