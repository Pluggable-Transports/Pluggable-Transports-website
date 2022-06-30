---
title: "IETF April 2022 Monthly Report"
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

## April 2022 Monthly Report

By David Oliver of Guardian Project -
[david@guardianproject.info](mailto:david@guardianproject.info)

## MASQUE

The MASQUE Working Group reached consensus on both [HTTP/3
Datagrams](https://datatracker.ietf.org/doc/draft-ietf-masque-h3-datagram/) and
[Connect-UDP](https://datatracker.ietf.org/doc/draft-ietf-masque-connect-udp/)
during April, allowing higher-level review of these drafts before publication
as Recommendations. This is a major milestone since these two protocols
represent the core of the modern MASQUE ideas (together, allow proxying of both
QUIC and IP datagrams over HTTP connections). The
[Connect-IP](https://datatracker.ietf.org/doc/draft-ietf-masque-connect-ip/)
draft will complete the vision: proxying of all types of IP-based traffic of
HTTP/3 (QUIC).

As we’ve said in our previous reports, we still hold out hope for [HTTP
Transport
Authentication](https://www.ietf.org/archive/id/draft-schinazi-httpbis-transport-auth-05.html),
though the draft has currently expired. The foremost reason for this is that
the author is now on the IETF’s Architecture Board and chairs two other working
groups.

A new MASQUE-related draft was created related to finding services in a MASQUE
environment - 
[HTTP Access Service Description Objects](https://sandbox.ietf.org/doc/draft-schwartz- masque-access-descriptions/).
This draft describes how to find a MASQUE server if (1) software is starting
with an HTTP CONNECT proxy or (2) use of multiple services in combination (e.g.
CONNECT-UDP + CONNECT-IP + DoH + ...) is required. According to the author, the
design could also serve as a building block for a solution to the key
consistency problem in Oblivious HTTP, described in Key Consistency for
Oblivious [HTTP by
Double-Checking](https://datatracker.ietf.org/doc/draft-schwartz-ohai-consistency-doublecheck/).

## Messaging Layer Security (MLS)

The MLS Working Group submitted its call for consensus on draft #14 of the [MLS
protocol](https://datatracker.ietf.org/doc/draft-ietf-mls-protocol/) document
on May 3, 2022. While this will likely get some “editorial” commentary, it
appears the technical content of this draft is ready for recommendation to IETF
as a standard.

A potential blocker: one of the Working Group chairs filed an [Intellectual
Property Rights disclosure](https://datatracker.ietf.org/ipr/4015/) (as
procedure requires) on the protocol draft. The WG will have to deal with this
in list discussion or in an interim meeting, and come up with a formal
resolution/response if the draft is to proceed.

A virtual interim meeting on the protocol draft is scheduled for June 2, 2022.

## Privacy Pass

Two updated drafts have been released:
1. [draft-ietf-privacypass-protocol-04.txt](https://datatracker.ietf.org/doc/html/draft-ietf-privacypass-protocol-04.html)
and
2. [draft-ietf-privacypass-architecture-03.txt](https://datatracker.ietf.org/doc/html/draft-ietf-privacypass-architecture-03.html)

Mailing list discussion indicates there is still considerable work to do in
both areas, however Apple updated its draft [Privacy Pass HTTP Authentication
Scheme](https://datatracker.ietf.org/doc/draft-pauly-privacypass-auth-scheme/)
(such a scheme is required for client interaction with the Privacy Pass
protocol). Does this seek to standardize work Apple has already experimented
with in its iCloud Private Relay? Pure speculation.

A new draft was submitted relating to work - discussed at IETF113 - on rate
limiting. [Rate-Limited Token Issuance
Protocol](https://www.ietf.org/archive/id/draft-privacypass-rate-limit-tokens-01.txt).

## Oblivious HTTP (now Oblivious HTTP Application Intermediation, OHAI)

As above, a new draft ([Key Consistency for Oblivious HTTP by
Double-Checking](https://datatracker.ietf.org/doc/draft-schwartz-ohai-consistency-doublecheck/))
was submitted to the OHAI working group after IETF113 discussion about the
relationship between key consistency and key discovery. The author felt it is
natural to combine discovery and consistency-checking, this draft exploring how
that could work. Note that this technique relies on the Oblivious Proxy also
being a MASQUE server, so that the client can fetch the KeyConfig from both the
proxy (providing consistency) and the origin (providing authenticity).
Interesting aggregation of technologies. This draft will provide room for
debate on at least some of the key-related issues in OHAI.

The core OHAI draft has not advanced further this month.

## Privacy Preserving Measurement (PPM)

After what was thought to be a thorough (but many felt confusing)
[presentation](https://datatracker.ietf.org/meeting/113/materials/slides-113-ppm-ppm-overview/)
on PPM at IETF113, and an [update to the original
draft](https://datatracker.ietf.org/doc/draft-gpew-priv-ppm/01/) submitted
mid-month, a yet-newer draft has been submitted by the principals: [Distributed
Aggregation Protocol for Privacy Preserving
Measurement](https://www.ietf.org/archive/id/draft-ietf-ppm-dap-00.html) to
replace those original submissions, after a lengthy discussion on the mailing
list. There is clearly a long way to go on this effort - the overall trust
model not being well understood nor (as a constituent problem) authentication
between parties.

## IETF114

IETF has announced that IETF114 will go forward as planned in Philadelphia PA,
July 25-29 2022 with the Hackathon running July 23-24. I will attend both
events.
