---
title: "PTIM 2021 Day 3 Recap"

modified: 2021-12-29
categories:
  - blog
tags:
  - implement
  - developer
  - meeting
  - virtual
layout: posts
---

**This year’s Pluggable Transport Implementers Meeting was a huge success! Thank you to all the speakers and everyone who attended. For those of you who missed it or would like a refresher, here is a recap of day three**



To kick off the final day of the PTIM, we began with a session on Internet Freedom in the MENA region. The presenter, an expert on internet freedom in the Middle East, outlined the global decline in Internet Freedom and highlighted the main patterns leading to this outcome. In many cases, the governments lashed with tech companies on users’ rights as they pushed to regulate the industry to subdue free expression and gain access to private data. Specifically in Iraq, people will carry out cyberattacks to destroy activists’ reputations. For example, an Iranian backed militia used Telegram to offer a cash prize for personal information about activists. Another notable pattern is the lack of oversight by big tech companies in the MENA region. Many rely on AI for content moderation which misses things or makes mistakes, making content moderation a huge issue in the region. In general, the MENA region has issues with accessibility, accountability, and protection when it comes to Internet Freedom. 

The next session pivoted to the policy side of circumvention with a presentation titled “Policies Policing your Codes and How We Use Them”. Countries like Bangladesh and Tanzania have enacted digital security laws in the past that make expressing yourself online illegal and are used to target journalists. Other governments require bloggers and activists register for a license and make it illegal to download, distribute, use, or develop VPNs. Beyond government regulations, policies from tech companies also limit developers. For example, data protection and privacy policies for companies like Facebook in Ethiopia and Myanmar don’t necessitate informed consent practices. In order to avoid and fix these issues, developers need to participate in policy conversations around digital rights. As these policies often effect developers and their code, they are an important piece of the conversation. There are also resources available such as Freedom on the Net report, Access Now, The Web Foundation, IFF’s Digital Rights Community, and more. 

To continue the policy and regional theme, the next session as a deep dive into censorship in Cuba. Cuba is infamous for censorship and every year scores within the bottom ten for internet freedom, more similar to countries further away in terms of internet repression. In Cuba, there is only one telecom provider known as ETECSA which is owned by the government. In general, they provide bad quality, extremely expensive services and are known for surveillance and punishment. Since 2018, most Cubans use data plans on mobile phones to get online. There are about 5 million internet users out of the total 11.2 million population. 94% of households do not have internet at home, highlighting the use of mobile phones for internet access. The governments main concern surrounding internet use is the spread of information, especially information citizens use to organize protests and other anti- government sentiments. Censorship tactics include internet disruptions, targeted suspensions for specific individuals, blocking SMSs that contain keywords, arrests and interrogations, disinformation campaigns, and new laws to regulate online behavior. The government has increasingly relied on targeted suspension when protests began popping up and they focus on targeting leaders of movements. In addition, there is a lack of tech literacy and people are not aware of dis or mis information they are taking in and are not aware of things like VPNs. The government rant a disinformation campaign around VPNs to discourage people from using them and misrepresenting the use of circumvention tools. Overall, there is a lot of progress to be made but difficult to influence the Cuban government to change their practices. 

Continuing the censorship conversation, we pivoted to the technical side of censorship with a session titled “Geneva and Tunnelbear: Packet Manipulation Evasion at Scale”. Traditionally, evasion techniques are utilized in a manual manner, creating a cat and mouse game between those who censor and those who work against it. Geneva and Tunnelbear aim to automate this process and create a more efficient process for evasion. Geneva runs strictly at one side and manipulates packets packets to and from the client. The partnership with Tunnelbear allows Geneva to test and integrate these practices with their code. Both teams continue to work together to run performance measurements, use bottlenecks to determine code fixes, and more.

Next, Operator Foundation presented their Shadow Transport. Shadow is a wrapper for Shadowsocks that makes it available as a pluggable transport. It’s written in Swift, Java, Kotlin and GO. It works in a lot of different places and is usable without a command proxy. The Shadow Transport provides the same defense as shadowsocks.

To wrap up the final day of the PTIM, people chose to “continue the conversation”. This led to fruitful conversations about censorship in Cuba and allowed people to brainstorm ways to help. Given the different expertise and set of skills among the participants, many planned collaborative efforts to work with each other on topics ranging from VPN usage and specific censorship strategies in Cuba. 

Thanks to the participants, speakers, and the general community, the 2021 PTIM was a huge success.  Through highlighting the work going on in this community and bringing in new voices and perspectives, the PTIM allowed the community to grow and make room for new collaboration. We thank everyone for their contribution and look forward to holding more events!
