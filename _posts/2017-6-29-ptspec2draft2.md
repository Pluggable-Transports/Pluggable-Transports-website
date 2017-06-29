---
title: "New Pluggable Transport Specification Version 2.0, Draft 2"
modified: 2017-06-29T13:30:02-05:00
categories:
  - blog
tags:
  - PTSpec
  - IPC
  - Go
  - API
  - developer
layout: archive

---
The process of updating the Pluggable Transport Specification Document (PT Spec) is still ongoing. Recently Dr. Brandon Wiley of the Operator foundation released an updated version 2.0 Draft 2, of the Spec which answers different issues in the previous spec. 
In the new draft, new terminologies were introduced and others were removed to offer more clarity and to differentiate between numbers of PT Spec elements. Unused examples were removed and others were improved with cleaner code and better descriptions. 

##### The new Spec’s changelogs from Draft 1 are:

● Renamed version flag to ptversion to avoid naming conflict with goptlib

● Modified Go examples to use correct Go syntax

● Renamed pt module in Go examples to base to avoid naming conflict with goptlib

● Reworded introduction

● Clarified Go examples with more details on how to implement a transport in Go

● Removed unused Javascript and Python APIs

● Removed SSH transport example

● Standardized use of Transports API and Dispatcher IPC language throughout

● Added length to per-connection parameter encoding

Currently the Network Traffic Obfuscation community is discussing the changes under this thread https://groups.google.com/forum/#!topic/traffic-obf/sfDgcZk8s3s  
in order to collect feedback and reflect them on the final version of the Spec which is going to be released before the end of this year (2017).

You can find the new PT Spec here: 

https://www.pluggabletransports.info/spec/pt2draft2 

