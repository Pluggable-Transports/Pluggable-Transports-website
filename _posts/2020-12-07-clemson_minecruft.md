---
title: "PTIM 2020 Guest Blog - Multipath TCP and Minecruft"
modified: 2020-12-07T13:20:02-05:00
categories:
  - blog
tags:
  - transport
  - PTIM2020
  - Conference
  - Virtual
  - Minecruft
  - Multipath
  - TCP
layout: posts

---
 This guest blog, from Clemson University, discusses the progress made on the team's most recent PT technologies. 


## Multipath TCP and Minecruft 
_Dr. Richard Brooks, Clemson University_


My research group at Clemson has created a number of PT technologies. This blog post discusses two recent PT advances: Multipath TCP and Minecruft. Though both advances are very different, they each hold promise for widening PT adaptation. 


### Multi-Path TCP


The Internet, as we know it, is defined by a set of network protocols known collectively as Internet Protocol \(IP\). Probably, the most widely used member of IP is the Transport Control Protocol \(TCP, or TCP\/IP\). TCP provides error free delivery of a stream of data from a source computer to a destination computer. TCP verifies that data is delivered to the destination in the same order that it left the host, and certifies that the data is complete.


Unfortunately, TCP/IP has a number of drawbacks for the PT community:

* Latency can be a problem. Researchers at the Harvard Berkmann center found user perceived latency to be the most important factor for users that decide not to use privacy enhancing networking tools.
* Source and destination addresses are in clear text, which makes it easy to identify information sources accessed, and the networking tools used.
* The TCP/IP data stream is a linear sequence of packets, which can compromise the privacy provided by privacy enhancing technologies. Web pages being read can be identified by the number of connections made and the volume of traffic in the stream. This is called web site fingerprinting.


A recent alternative to standard TCP/IP is multi-path TCP \(MP-TCP\). MP-TCP provides the same end-to-end functionality of TCP\/IP. The difference is that MP-TCP divides the TCP\/IP connection at the source into multiple data streams. In a MP-TCP connection, the number of active data streams going over the Internet can vary. Each session can take a different path through the Internet, although all sessions will have the same source and destination nodes. As with traditional TCP/IP, the MP-TCP data streams are reassembled into the same data stream that left the source at the destination.


While MP-TCP is not, strictly speaking, a PT, we found that integrating MP-TCP with PTs has a number of advantages. We have deployed and tested the following configurations:


* PT multi-session, but not multi-path: divides the TCP/IP PT session into multiple different sessions that all take the same path.
* PT session divided into multiple paths: takes a PT session, divides a PT session into a number of different paths going from source to destination.
* MP-TCP session going through PT\(s\): each separate path of an MP-TCP session is camouflaged using PT\(s\).
* MP-TCP session secure tunnel\(s\) going through PT\(s\): each separate path of a MP-TCP connection is encapsulated in a secure VPN, which is then camouflaged using PT(s).


All of these configurations worked. In each configuration, the MP-TCP version reduced latency when compared with an equivalent session that used standard TCP. All of the approaches were somewhat effective against website fingerprinting, with the first approach being the least effective. The first two configurations do not hide the fact that MP-TCP is being used. At the moment this is not an issue, sinceMP-TCP is not strongly associated with privacy tools. Should MP-TCP become widely used in our community, this could become an issue.


Of most interest, and the most secure, is the fourth configuration that we tested. It has the following useful properties:
* Network monitors will not be able to identify that MP-TCP is used,
* If intermediate nodes are outside of the are controlled by a network
*  monitor, intermediate nodes hide the final network destination,
* MP-TCP adapts automatically to send traffic over paths with more
*  bandwidth, so the connection continues even when paths fail,
* Each path can use a different PT. This allows a network connection to
*  test multiple PTs at once. The session works as long as any PT is not
*  blocked,
* MP-TCP is faster than any equivalent connection not using MP-TCP.


We would be happy to help anyone else in the community configure and
test this approach.


### MineCruft


My research group has done a lot of work to both identify and hide network usage. This includes format transformation encryption \(FTE\), which takes one network session and encodes/decodes it into/out of another protocol while passing through the network. Network monitors can then be fooled, since the user does not appear to be doing anything suspicious.


One challenge with this is that the "host" protocol which is used for FTE encoding has to be:
* Widely used,
* Innocuous,
* Consume lots of bandwidth, and
* Be something network monitors will not want to block.


Which sounds like video games to me. Gamers get really annoyed when someone stops them from playing.Among the most popular and well known games, Minecraft turns out to be an excellent choice. It is one of the most widely used games worldwide and it is often used in educational settings. The game’s aesthetics are also relatively innocuous, which allows it to be accepted among a wide array of different cultures.   Perhaps most significantly, Minecraft is widely "modded,” which mean it can be easily modified by individuals to fit their needs. Consequently, individuals can run their own servers and choose between “online” or “offline” modes, the latter of which does not require log-in credential from the game owner. Mojang, the owner of Minecraft, supports all of these options.


We implemented a PT that encodes browser traffic into valid Minecraft user sessions. Browser traffic is sent through a socks proxy, which produces a high entropy bit stream. We then map bit positions to Minecraft move parameters that can be set by the user. On the other side, this method is reversed. 


After the initial implementation, we held a hackathon in Dakar with members of the Africtivistes NGO. Together, we spent one week onxtending the original implementation. The first challenge was adapting the code, which had been tested in North America, to run on the African networks. Africa's networks have very little wired infrastructure with their own unique dynamics. We encoded new moves for the translation, which increased network throughput by almost 20%.


The Minecraft based PT, which we call Minecruft, exists as a proof of concept. We hope to extend it in the near future.