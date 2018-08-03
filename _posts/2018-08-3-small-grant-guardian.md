---
title: "Small Grant Announcement - Guardian Project"
date: 2018-08-03
modified: 2018-08-03

categories:
  - blog
tags:
  - research
  - implementation
  - developer
  - grants
layout: posts

---
More good news from the Pluggable Transports team as we announce some new funding for [Guardian Project](https://guardianproject.info/), coming from our ongoing Small Grants pool.

This project will enable traffic obfuscation on the Android platform using Snowflake, updating their [PLUTO2](https://github.com/guardianproject/AndroidPluggableTransports) code for developers. Guardian Project will also integrate Snowflake directly into [Orbot](https://guardianproject.info/apps/orbot/), making it easy for Android Tor users to select a compatible bridge. To achieve these goals, the team will be implementing an alternate bootstrapping method for Snowflake, changing the way it currently relies on domain fronting.

[Snowflake](https://github.com/keroserene/snowflake) is a WebRTC-based Pluggable Transport, combining the advantages of flashproxy and meek. It is designed to be able to support many more users at lower cost than a meek deployment. Once running, it uses volunteer proxies without the need for manual port forwarding. As part of this project, Guardian will also be investigating ways of increasing Snowflake volunteers.

Remember, if you want to submit an idea for a small grant, visit [this page](https://www.surveymonkey.com/r/pluggabletransports). Applications are being reviewed on a rolling basis, we're looking for both implementation projects and research into new pluggable transports.