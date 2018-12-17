---
title: "Briar messaging - now with PT support!"
date: 2018-12-04
modified: 2018-12-04

categories:
  - blog
tags:
  - implementation
  - developer
  - grants
layout: posts

---
A couple of months ago, we announced a partnership with [Sublime Software](https://briarproject.org) to build support for Pluggable Transports into their messaging app, [Briar](https://play.google.com/store/apps/details?id=org.briarproject.briar.android). The latest beta release of the app now comes with Pluggable Transports built in, giving you more chance of successfully communicating over the Internet wherever you are.


If you're new to Briar, it's a messaging app that is designed for safety and resilience. It can continue working even when the Internet is unavailable, as it also works over Bluetooth and local WiFi, and there is no central message store. All of your communications are synchronized between the devices you exchange messages with.

Using Pluggable Transports for accessing the Internet is simple. In the Settings menu, you would choose to *Connect via Internet \(Tor\)*. In most cases, the *Automatic* setting will connect you via Pluggable Transports if they are needed, but you can also choose to *Use Tor with bridges* if you need to force that option.

<img src="/assets/images/briar-settings.png" alt="Briar settings screen" style="margin: 0px 20px"/><img src="/assets/images/briar-bridges.png" alt="Briar Tor options" width="300"/>
