---
title: "PTIM 2021 Day 1 Recap"
modified: 2021-11-05
categories:
  - blog
tags:
  - implement
  - developer
  - meeting
  - virtual
layout: posts
---

## This year’s Pluggable Transport Implementers Meeting was a huge success! Thank you to all the speakers and everyone who attended. For those of you who missed it or would like a refresher, here is a recap of day one:

The day started with a session titled “Sharing Best Practices for Uncovering Surveillance Middleboxes” presented by Valentin Weber and Vasilis Ververis. They shared their work on trying to find a methodology to detect middleboxes and discover any malicious activities. Using open data sources, they worked to find technical ways to find middleboxes. They found middleboxes being used in SafeCities and for blocking. It is probable that middleboxes are being used for censorship and if are not yet, they will in the future. 

The next session was a collection of lightning talks. The first talk titled “Snowflake: Challenges growth and censorship attempts” began with an overview of the pluggable transport “Snowflake”. The presenter then shared use cases of Snowflake and key censorship events in which it was used such as in China in May 2019. In the future, developers will continue to make Snowflake code pluggable and usable and find alternatives to domain fronting. 

The next talk titled “Thoughts on Measuring Pluggable Transports” was presented by Simone Basso of OONI. This presentation focused on the measurement side of pluggable transports. OONI measures censorship events around the world and collects data from these events that goes on to inform many people including activists, developers, and more. The discussed the ways OONI tests to see if PTs are working and the projects they are working on using and testing PTs. 

The next session titled “TunnelBear Bandwidth Support Program” outlined the work TunnelBear has been doing in recent years and their involvement in the internet freedom support space. They created a bandwidth support program to be used during censorship events. This [program provides free limited bandwidth upgrades to countries facing widespread online censorship. This helps users access the internet in crucial times in which communication is needed. They also report on VPN availability during blocking events. 

Awn Umar presented a lightning talk titled “Rosen: An easy to use, modular, obfuscating proxy tunnel”. Starting off as an effort to make Open VPN harder to block, Umar created Rosen. The package implements a modular framework for proxies that encapsulate traffic within some cover protocol to circumvent censorship based on deep packet inspection and endpoint fingerprinting techniques. You can find more [here](github.com/awnumar/rosen). 

A member of the Internet Society presented “Measuring the Health of the Internet with the Pulse Platform” to provide an overview of Internet Society’s Pulse Platform. The platform monitors shutdowns around the world and measure data such as cable cuts, how many endpoints there are and put it together country by country. At the moment, they are collecting data mostly from African regions, aiming to expand their data collection. The end goal is to aggregate as much data as possible about the health of the internet. You can learn more [here](pulse.internetsociety.org). 

The day closed with two final sessions, one covering Sub Saharan Africa Internet freedom issues and the other titled “Building a Successor for Obsf4”. Presented by Dr. Richard Brooks and Sub Saharan Africa Activists, the first session outlined the internet freedom issues plaguing the region such as shutdowns, laws making internet less accessible, arrests of journalists, etc. Elections in the region have been marked with internet shutdowns and targeted attacks against prominent internet activists. The activists highlighted the importance of Internet access for information sharing and growth of the economy. VPN use has increased in the last decade and is more widely known than before. The challenge is that many people use free VPNs that have its own set of challenges. The discussion following the presentation covered the technical needs of activists in the region and plans for collaboration. 

Brandon Wiley of Operator Foundation presented the idea of building an successor for obfs4. Obfs4 is a very popular and widely deployed transport and there are several reasons to think about a successor such as maintainability of the codebase to increasing blocking resistance. The scope of the design is a “looks like nothing” type design with hopes od reducing number of dependencies. Implementing it on every platform, and a common multi-platform cryptography. 

To wrap up the day, attendees chose to “continue the conversation” and had fruitful discussions about topics like the PT specification and more on Sub Saharan Africa Internet Freedom. 


