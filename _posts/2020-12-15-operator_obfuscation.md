---
title: "PTIM 2020 Guest Blog - Obfuscation Is Not Security"
modified: 2020-12-15T13:20:02-05:00
categories:
  - blog
tags:
  - transport
  - PTIM2020
  - Conference
  - Virtual
  - Obfuscation
  - Operator
layout: posts

---
 This guest blog, from the Operator Foundation, weighs the role obfuscation plays in encryption.

## Obfuscation: Why It Does Not Mean Security
_Adelita Schule, The Operator Foundation_

The key to Pluggable Transports is obfuscation. This is the mechanism by which we help our users get where they need to go by letting their traffic blend in with traffic that would not normally be blocked (or possibly by making their traffic not look like anything worth paying attention to). Obfuscation is a disguise; it is misdirection, sleight of hand. What you see is not what you think you see, but that is all. Misdirection should not be confused with security.


Like most disguises, obfuscated network traffic may not evade scrutiny.  If your traffic is scrutinized, it is only private if you have also taken specific steps to make it secure with robust encryption. Encryption is a fantastic way to make data look like something else. Encryption is not only for strict security, and anything that makes employing a cipher more accessible could be used. Even a cipher that now has known vulnerabilities would still work well for obfuscation purposes. If a transport uses a cipher, you should assume it is for obfuscation and not security unless the developer explicitly states otherwise.


The need for internet security is ever more important in our connected world, and many of us who spend time working to increase internet access also work to improve internet security. The distinction between what a pluggable transport does and does not do is one that can easily be misunderstood, especially when the transport in question is known to use an encryption cipher. We all know how important security is, so it’s important that we also remember that pluggable transports are primarily meant to obfuscate and not necessarily to maintain security.