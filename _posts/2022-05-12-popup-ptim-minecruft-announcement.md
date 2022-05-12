---
title: "Pop Up PTIM Session: Gamer Games Unblocked"
modified: 2022-05-12
categories:
  - blog

tags:
  - implement
  - developer
  - meeting
  - virtual

layout:
  - posts
---

# Gamer Games Unblocked

## Presented by Dr. Richard Brooks

We are excited to announce the next Pop Up PTIM Sessions! These are stand alone
sessions, allowing the community to discuss PT related topics and stay up to
date on the progress of this community.

Our second session will be presented by the Dr. Richard Brooks of Clemson
University. They will share the work they have been doing for the past few
years on the usage of PTs in the context of a VPN application.

This session will take place on **May 19th at 13:00 UTC/9:00 ET**.
Please use [this link](https://cryptpad.fr/form/#/2/form/view/GdwbPhD-Cpctzu8WAvVCSdH+G-KMawkttEagyT2Ot0c/) to register for the event. 
This takes no more than 2 minutes!

Read more about it below:

Pluggable transports (PTs) provide access to the global Internet by translating
Internet communications (mainly browser traffic) into internet protocol (IP)
sessions that the censoring agents are unwilling to interrupt. It should be
difficult to differentiate the PT traffic from other sessions that our
adversaries (censors) normally encounter. The sessions we mimic should ideally
have high bit-rates. Blocking our PT sessions should impose an unacceptable
cost on the censor.

Young people play video games. Technically literate young people are even more
likely to be gamers. Many countries that control access to information want to
have their own technical capabilities, making them dependent on their young
technical workforce. This makes these countries unlikely to block gaming
sessions. Since invasive monitoring of gaming sessions slows sessions and hurts
the gaming experience, we have been informed that network monitors leave
traffic alone once they have identified it as a video game session.

This session will look at our implementation of a PT that encodes IP traffic as
Minecraft video game sessions. The planned agenda for this session will be:

- A very brief and shallow introduction to Minecraft,
- An explanation of aspects of Minecraft that make it particularly attractive
  for PT use,
- Use of Wireshark, with a plug-in to inspect Minecraft IP sessions,
- An explanation of the architecture of our Minecraft based Minecruft PT,
  including our use of Docker to minimize the amount of IP stack configuration
  nflicted upon users,
- An introduction to the quarry Minecraft protocol library we leverage, and
  documentation of Minecraft internals,
- Explanation of our Python implementation and how we use Python’s functional
  programming to generate packet encoding/decoding functions on the fly,
- A live demonstration of a Minecruft session using netcat for system input and
  output, and
- Illustration of how a machine learning approach is used to generate traffic
  that conforms to the behaviors of observed game sessions.

We are excited to see you all there!
