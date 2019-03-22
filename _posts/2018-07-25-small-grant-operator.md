---
title: "Small Grant Announcement - Operator Foundation"
date: 2018-07-25
modified: 2018-07-25

categories:
  - blog
tags:
  - research
  - implementation
  - developer
  - grants
layout: posts

---
We are pleased to announce that [Operator Foundation](https://operatorfoundation.org) is the first recipient of our latest Small Grants pool!

The Operator team will be building on previous Protean implementations for Javascript and Go, porting the most relevant subset of the network transformations from Go to Swift. This work will essentially create a new transport that is compatible with the Pluggable Transport Swift API, allowing use on Apple's iOS.

Protean is a collection of UDP-to-UDP packet transformation tools. From Operator's [README](https://github.com/OperatorFoundation/protean/blob/master/README.md): *The overall goal of Protean is to provide transformations from UDP traffic into other UDP traffic, where the target UDP traffic has properties that resist network filtering. This is in contrast to tools such as Shapeshifter Dispatcher, which provide resistance to network filtering by tunneling UDP traffic over TCP protocols.*

The target audience for the new project is iOS applications that use UDP protocols for communication, and will be of particular benefit for voice and video communication applications.

You can see more of Operator Foundation's projects on [Github](https://www.github.com/OperatorFoundation), and we'll update this blog when the project is completed.

If you want to submit an idea for a small grant, visit [this page](https://www.surveymonkey.com/r/pluggabletransports). Applications are being reviewed on a rolling basis, we're looking for both implementation projects and research into new pluggable transports.