---
title: "New Pluggable Transports Specification 2.1"
date: 2018-09-30
modified: 2018-09-30

categories:
  - blog
tags:
  - implementation
  - developer
  - PTSpec
  - Dispatcher
  - Go
layout: posts

---
The latest version of the Pluggable Transports specification has been released, and is available [here](https://github.com/Pluggable-Transports/Pluggable-Transports-spec/tree/master/releases/PTSpecV2.1Draft1).

A major change in this revision is that it has been moduralized, so that components can be upgraded independently. The new spec consists of the following documents:

* Base specification v2.1
* Dispatcher IPC Interface v2.1
* Go Transport API v2.1 - This has the most changes, all based on requests from the community.
* Swift Transport API v1.0 - This is brand new, created to allow developers to deploy Pluggable Transports in an iOS or macOS application, or as a Network Extension.