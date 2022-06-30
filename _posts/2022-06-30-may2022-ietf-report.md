---
title: "IETF May 2022 Monthly Report"
categories:
  - blog
tags:
  - implement
  - developer
  - IETF
layout: posts
---

In our IETF series, we have contributions from David Oliver of [Guardian
Project](https://guardianproject.info). David has attended several
[IETF](https://ietf.org) meetings, learning about new protocols that can help
advance Pluggable Transports work and benefit censorship circumvention
developers.

## May 2022 Monthly Report

By David Oliver of Guardian Project -
[david@guardianproject.info](mailto:david@guardianproject.info)

## MASQUE

In April, the MASQUE Working Group reached consensus on both [HTTP/3
Datagrams](https://datatracker.ietf.org/doc/draft-ietf-masque-h3-datagram/) and
[Connect-UDP](https://datatracker.ietf.org/doc/draft-ietf-masque-connect-udp/),
allowing higher-level review of these drafts before publication as Proposed
Standards. In May, two of those reviews are nearing completion, three more are
requested. Barring major issues - unexpected - these will close in June. As we
suggested in April, this is a major milestone since these two protocols
represent the core of the modern MASQUE ideas (together, allow proxying of both
QUIC and IP datagrams over HTTP connections).
[Connect-IP](https://datatracker.ietf.org/doc/draft-ietf-masque-connect-ip/) -
which will complete the vision - remains in its initial draft state while the
work above completes.

Here’s one I’ve missed: the second draft of [HTTP Datagram PING and
TIMESTAMP](https://datatracker.ietf.org/doc/draft-schwartz-masque-h3-datagram-ping/)
was posted at the end of May. This draft defines new mechanisms for measuring
the functionality and performance of an HTTP Datagram path, matching protocol
artifacts already available for TCP measurement. These mechanisms can be used
with CONNECT-UDP, CONNECT-IP, or any other instantiation of the Capsule
Protocol from the HTTP/3 Datagrams design.

## Messaging Layer Security (MLS)

As mentioned last month, draft #14 of the [MLS
protocol](https://datatracker.ietf.org/doc/draft-ietf-mls-protocol/) document
was submitted for Working Group Last Call on May 3, 2022.  A virtual interim
meeting on the protocol draft was held on May 26, 2022 and a number of issues
were raised and submitted as Github issues and [pull
requests](https://datatracker.ietf.org/doc/minutes-interim-2022-mls-05-202205261000/).
Many have now been resolved. During that meeting the protocol’s AppAck
extension was scheduled to be removed, in favor of a new generalized extensions
draft. This now exists as [The Messaging Layer Security (MLS)
Extensions](https://datatracker.ietf.org/doc/draft-robert-mls-extensions/).

Federation is back on the menu! Given *popular demand*, a slightly modernized
version of the original federation draft has been submitted, removing parts
that have become outdated in the intervening 2 years, and fleshed out existing
parts where more clarity now exists in the protocol draft itself. The author
states that this document merely gives a high-level overview and serves as a
starting point for discussion. It’s (currently) not a normative document so,
like the architecture document that kicked off the MLS protocol work, this
document could act as an umbrella document, referencing more other
more-concrete specifications. Here’s the new
[draft](https://datatracker.ietf.org/doc/draft-ietf-mls-federation/).

The Working Group agreed to update its milestones as well, putting some
pressure on themselves to finalize this work. The WG has decided to submit the
now-expired [MLS
Architecture](https://datatracker.ietf.org/doc/draft-ietf-mls-architecture/)
document as an Informational document (rather than a Proposed Standard) with
milestone of September 2022. The milestone for final submission of the [MLS
Protocol](https://datatracker.ietf.org/doc/draft-ietf-mls-protocol/) document
was also set at September 2022. This leaves one more IETF in-person meeting to
hash out the final details before submission for higher review.

## Privacy Pass

The core drafts were not updated in May, and there was limited discussion on
GitHub. [Rate-Limited Token Issuance
Protocol](https://www.ietf.org/archive/id/draft-privacypass-rate-limit-tokens-01.txt)
was refreshed with a draft #2. Good thing! The latest change fixes a replay
attack in the issuance protocol. From the authors:

> The rate-limited issuance protocol is a three-party protocol between Client,
> Attester, and Issuer. The input is a challenge from the Origin and the output
> is a token bound to that Origin. We found an issue that would allow a malicious
> Attester (or an attacker who compromises the Attester<>Issuer channel) to
> replay a Client token request and receive a valid token response for a
> different Origin.

## Oblivious HTTP (now Oblivious HTTP Application Intermediation, OHAI)

Two drafts have been updated by the Working Group: [Discovery of Oblivious
Services via Service Binding
Records](https://datatracker.ietf.org/doc/draft-pauly-ohai-svcb-config/)
(well-named!) and [Oblivious Proxy
Feedback](https://datatracker.ietf.org/doc/draft-rdb-ohai-feedback-to-proxy/)
(feedback on rate-limiting, at least initially). These drafts flesh out the
original proposal to handle situations already arising in interop testing and
development. 

The core OHAI draft has not advanced further this month. Frankly, I’m not sure
why this seems to have stalled.

## Privacy Preserving Measurement (PPM)

[Distributed Aggregation Protocol for Privacy Preserving
Measurement](https://datatracker.ietf.org/doc/draft-ietf-ppm-dap/) was made an
official working group document in early May. There are eleven new issues and
nine pull requests on this version of the document to date. Mailing list
discussion still centers on understanding the key ideas (as opposed to debating
the details).

[STAR: Distributed Secret Sharing for Private Threshold Aggregation
Reporting](https://datatracker.ietf.org/doc/draft-dss-star/), initially
submitted in March, offers a different approach to solving the same problem.
