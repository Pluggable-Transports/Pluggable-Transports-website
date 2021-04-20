---
title: "Operator's Optimizer released for Swift and Go"
modified: 2019-12-30T13:20:02-05:00
categories:
  - blog
tags:
  - transport
  - library
  - Go
  - Swift
  - Operator
layout: posts

---
[Operator Foundation](https://operatorfoundation.org) recently completed and released two new prototypes for its Optimizer transport, for use with Go and Swift.

The Go prototype can be accessed from Operator's main Go transport repository at [https://github.com/OperatorFoundation/shapeshifter-transports](https://github.com/OperatorFoundation/shapeshifter-transports), while the Swift implementation can be accessed at [https://github.com/OperatorFoundation/Shapeshifter-Swift-Transports](https://github.com/OperatorFoundation/Shapeshifter-Swift-Transports). The two implementations have feature parity with regards to strategy modules, although the Swift version makes use of additional strategies designed for the Apple-specific environment.

In addition to this work, Operator have added Optimizer support to [shapeshifter-dispatcher](https://github.com/OperatorFoundation/shapeshifter-dispatcher), which allows Optimizer to be used in other environments, through the PT 2.1 Inter-process communication \(IPC\) specification. This extra development has also involved a modification to shapeshifter-dispatcher to allow options to be read from a configuration file rather than specified on the command line.

Full documentation for shapeshifter-dispatcher and the Optmizer transports are contained in the repositories' README files.