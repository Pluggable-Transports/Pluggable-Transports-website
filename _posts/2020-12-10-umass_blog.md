---
title: "PTIM 2020 Guest Blog - UMASS and Machine Learning"
modified: 2020-12-10T13:20:02-05:00
categories:
  - blog
tags:
  - transport
  - PTIM2020
  - Conference
  - Virtual
  - Machine Learning
  - UMASS
layout: posts

---
 This guest blog, from the University of Massachusetts, discusses the use of machine learning in fighting censorship. 

## Fighting Censorship with Machine Learning 
_Amir Houmansadr, University of Massachusetts, Amherst_

**The need for unobservable circumvention tools:** Recent studies by academics and practitioners suggest that today�s censors are capable of using advanced techniques (beyond traditional IP blocking and DNS interference mechanisms) to detect and block circumvention systems. In particular, new findings show the Great Firewall of China uses statistical measures and active probing mechanisms to detect, surveil, and block circumvention traffic of systems like Tor, ShadowSocks, etc. The continuing availability of low-latency, censorship-resistant communications thus critically depends on their unobservability.

 **Parrot circumvention systems:** The need for unobservable circumvention systems has motivated an entire class of circumvention systems that aim to achieve unobservability by _imitating_ popular applications such as Web browsers and Skype clients . Such systems are commonly referred to as parrot circumvention systems. For example, [SkypeMorph](http://bit.ly/HCKseX)  hides Tor traffic by mimicking Skype video calls and [CensorSpoofer](https://arxiv.org/abs/1203.1673) mimics SIP-based Voice-over-IP.

 Unfortunately, several studies have shown that such parrot systems can be easily identified and disabled by the censors using scalable, low-cost mechanisms, such as checking the entropy of a single packet per connection, or injecting probing packets into suspected connections. We argue that the entire approach of "unobservability by imitation" is fundamentally flawed, as convincingly mimicking a sophisticated distributed system like Skype, with multiple, inter-dependent sub-protocols and correlations, is an insurmountable challenge. To win, the censor only needs to find a few discrepancies, while the parrot must satisfy a daunting list of imitation requirements.

 **Hide-within (or parasite!) circumvention systems:** The challenges of building unobservable parrot systems motivated a successor class of circumvention systems, called hide-within (or parasite) systems. Similar to parrot systems, hide-within systems aim at resembling popular network protocols that are left unblocked by the censors. By contrast, as opposed to imitating the target network protocol, a hide-within system actually runs its target protocol, and tunnels circumvention traffic into the genuine runs of that target protocol. For instance, [FreeWave](http://citeseerx.ist.psu.edu/viewdoc/download?doi=10.1.1.451.7015&rep=rep1&type=pdf) establishes actual Skype calls (by running off-the-shelf Skype software) and encodes circumvention traffic into audio signals that are sent over the established Skype call (by leveraging a virtual sound card and acoustic modulation techniques). By virtue of tunneling into genuine instances of their target protocols, hide-within systems provide much stronger unobservability properties compared to the predecessor parrot systems.
However, hide-within circumvention systems suffer from two major issues, impeding their real-world deployments:

1.      Tunneling into another protocol: This is most-likely designed for a whole different purpose and is technically very challenging without introducing significant damage to the quality-of-service. For instance, circumvention traffic tunneled into Skype cannot exceed a goodput of more than 40Kbps, and the traffic tunneled into email messages will incur packet latencies in the order of seconds as well as  demanding CPU/battery overheads due to sophisticated encoding/decoding mechanisms.

2.      The act of parasiting may violate the terms of service of the host target systems, and therefore the approach seems unethical.

 **Filling the Gap Between Parrots and Parasites:** Our project aims to fill the gap between parrot and parasite systems. Our goal is to design circumvention systems that are:

1.      Practically deployable (similar to the parrot systems)

2.      Provide plausible unobservability against real-world censors, as opposed to the bullet-proof unobservability of the inefficient, unethical parasite systems.

To that aim, we plan to leverage advanced machine learning generator algorithms, such as GANs and transformers, to _automatically_ generate synthetic circumvention traffic that is practically indistinguishable from popular uncensored protocols -- without tunneling into such protocols. Our preliminary work has designed a technique that generates synthetic traffic whose features cannot be reliably distinguished from genuine Skype traffic even if the censors leverage state-of-the-art ML-based traffic classifiers. We are extending our technique to not only imitate passive statistical traffic features, but the end-to-end system behavior of its target system.


