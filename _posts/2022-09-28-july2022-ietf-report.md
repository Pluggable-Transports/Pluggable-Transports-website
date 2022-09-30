---
title: "IETF July 2022 Monthly Report"
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

## July 2022 Report

By David Oliver of Guardian Project -
[david@guardianproject.info](mailto:david@guardianproject.info)

## TL;DR

The Internet Engineering Task Force (IETF) has rapidly scaled up
privacy-focused standards activity since mid-2019. The most important are:
[MASQUE](https://datatracker.ietf.org/wg/masque/about/), [Messaging Layer
Security](https://datatracker.ietf.org/wg/mls/about/), [Privacy
Pass](https://datatracker.ietf.org/wg/privacypass/about/), [Oblivious HTTP
Application Intermediation](https://datatracker.ietf.org/wg/ohai/about/),
[Privacy Preserving Measurementand](https://datatracker.ietf.org/wg/ppm/about/)
and [TLS 1.3](https://datatracker.ietf.org/doc/html/rfc8446) with [Encrypted
Client Hello](https://datatracker.ietf.org/doc/draft-ietf-tls-esni/). The main
privacy-focused advocacy groups within IETF are the [Privacy Enhancements and
Assessments Research
Group](https://datatracker.ietf.org/doc/charter-irtf-hrpc/) and the [Human
Rights Protocol Considerations Research
Group](https://datatracker.ietf.org/doc/charter-irtf-hrpc/).

The effort undertaken here has developed key relationships within IETF as well
as knowledge of its processes, brought knowledge of these activities into the
Internet Freedom community, and engaged actively in the standards process with
the definition and implementation of [HTTP Transport
Authentication](https://datatracker.ietf.org/doc/draft-schinazi-httpbis-transport-auth/07/)
which the [HTTPbisWorking](https://datatracker.ietf.org/wg/httpbis/about/)
Group is now considering for adoption. More of these implementation efforts -
cooperations among the Internet Freedom community, the open source community
and the IETF - are recommended along with continued participation in its
ongoing activities.

## Introduction

Let's review why we thought this work might be important.

The majority of Internet Freedom funding is focused on solving immediate-term
problems stemming from active surveillance, active suppression of Internet
access (or access to specific content) and broad inequities in the availability
of Internet access around the world. Solutions with short-term and measurable
impact are sought. However, while this body of work has positively-impacted
many lives, it's safe to say that the hoped-for impacts have not been *at
Internet scale* - euphemistically molehills, not mountains. Standards bodies,
by contrast, operate at Internet scale and address problems in ways that can
produce highly-generalized and durable solutions. However, because standards
bodies are *deliberative*, results are not delivered in a timely fashion (or
sometimes not at all). The question was asked: If a small amount of Internet
Freedom funding was focused on steering Internet standards toward minimizing
unwanted surveillance, improving privacy and enhancing access around the world
could the gap be bridged between small-scale, short-term success and
large-scale, long-term success?

The Internet's two most important standards bodies are the Internet Engineering
Task Force ([IETF](https://www.ietf.org/)) and the World Wide Web Consortium
([W3C](https://www.w3.org/Consortium/)) - the former with focus on the
protocols that move content around the Internet, the latter with focus on the
content itself. The IETF, therefore, is the body most likely to produce
standards that impact our areas of interest.  Fortunately, given the long-held
spirit of cooperation among participants, the IETF has shown itself to be very
effective at delivering standards that (a) are defined in a manner agreeable to
the parties that must implement them and (b) have proven-interoperable
implementations demonstrating success *before* the standard is agreed.

Although IETF's interest in privacy didn't start in July 2019 (with the
[plenary talk](https://www.cs.columbia.edu/~smb/talks/ietf-privacy.pdf) at the
105th meeting of the IETF by Columbia's Steven Bellovin), IETF participants
have been running their privacy efforts in high gear since that time. In
addition to technical efforts such as [Privacy
Pass](https://datatracker.ietf.org/wg/privacypass/about/) and [Privacy
Preserving Measurement](https://datatracker.ietf.org/wg/ppm/about/), the
[Privacy Enhancements and Assessments Research
Group](https://datatracker.ietf.org/rg/pearg/about/) and the [Human Rights
Protocol Considerations](https://datatracker.ietf.org/rg/pearg/about/) Research
Group monitor, respectively, the most modern research in the implications of
technology on privacy and the human implications of the protocols IETF designs.
These close interactions among researchers, advocates and implementers have
created a strong feedback loop that improves the chance of long term success in
network protocol design. For example, [Transport Layer
Security](https://datatracker.ietf.org/doc/rfc5246/(https://datatracker.ietf.org/rg/pearg/about/)
now secures [over 99% of all Internet browsing
traffic](https://radar.cloudflare.com/). The most recent version, TLS 1.3, with
even better security and supporting the privacy-enhancing [Encrypted Client
Hello](https://www.ietf.org/archive/id/draft-ietf-tls-esni-14.html), already
accounts for 60% of Internet traffic, mostly via the [Chrome
browser](https://www.google.com/chrome/downloads/) (the Internet's most
popular). Development of the initial version of TLS and the current version
each took over four years to complete.  Here's [an excellent
article](https://dl.acm.org/doi/fullHtml/10.1145/3442381.3450057) describing
the process of bringing TLS 1.3 into broad adoption. An effort similar to TLS
(called [Messaging Layer Security](https://datatracker.ietf.org/wg/mls/about/),
expected to reach standardization in 2023) is having a similar gestation period
and could have TLS's level of industry-wide impact on the privacy and security
of messaging systems.

Within this context, we set out to discover if engaging with the IETF could
both inform our work solving the most immediate privacy problems of the
Internet and if our work could inform future IETF efforts to create durable and
widespread standards-based solutions. From our original proposal, this initial
foray was intended to:

* Identify standards proposals applicable to Internet freedom via engagement
  with IETF working groups and research teams, as well as individual
  participants as necessary
* Report on related new work and on the progress/status of existing work items
* Engage others in the Internet Freedom community to identify existing
  standards where work is necessary to correct failings or enhance Internet
  freedom

## Important developing standards

At any one time, the IETF is engaged in the development of hundreds of
standards - from small corrections or additions to existing standards (for
example, [management of traffic
congestion](https://datatracker.ietf.org/doc/draft-ietf-tcpm-accurate-ecn/) in
TCP) to major protocol upgrades (for example,
[QUIC](https://datatracker.ietf.org/wg/quic/about/)). While even seemingly
minor protocol nuances can have [huge future implications on privacy and
Internet
Freedom](https://mailarchive.ietf.org/arch/msg/quic/up8EhFHPIHMQbFK3yHVFV6JrZM0/)
(for example, the [QUIC Spin
Bit](https://tools.ietf.org/id/draft-trammell-quic-spin-00.html)), our efforts
were focused on major privacy-first *from-whole-cloth* activities.  Here's a
condensed description of these works and where they stand today.

### MASQUE

Multiplexed Application Substrate over QUIC Encryption
([MASQUE](https://datatracker.ietf.org/wg/masque/about/)) began life with a
small idea: provide a mechanism to co-locate networking applications -
specifically VPNs - behind an HTTPS web server in a manner that makes those
services indistinguishable from other HTTPS content and services to any
unauthenticated observer. The MASQUE definition grew much larger, however, when
it was realized that the definition of the QUIC and HTTP/3 protocols could be
enhanced with all forms of proxy flow, not just one narrow definition. The
original idea was hived off into a [separate
draft](https://tools.ietf.org/id/draft-schinazi-httpbis-transport-auth-00.html),
to await later discussion (more on this later), a Working Group was formed and
a suite of key standards was developed.

In April, the MASQUE Working Group reached consensus on both [HTTP/3
Datagrams](https://datatracker.ietf.org/doc/draft-ietf-masque-h3-datagram/)
(datagrams for HTTP) and
[Connect-UDP](https://datatracker.ietf.org/doc/draft-ietf-masque-connect-udp/)
(proxying datagrams over HTTP). In June, as expected, both drafts were
submitted for publication as Proposed Standards. The Working Group has now
commenced work on
[Connect-IP](https://datatracker.ietf.org/doc/draft-ietf-masque-connect-ip/)
(proxying IP packets over HTTP) which defines the last of the required proxying
scenarios agreed by this Working Group. Upon completion, all forms of Internet
traffic - IP, UDP, TCP and QUIC - will have an HTTP proxying definition. At
IETF114 in July, work began on interoperation testing for Connect-IP.

### Messaging Layer Security (MLS)

Messaging Layer Security ([MLS](https://datatracker.ietf.org/wg/mls/about/)) is
IETF's attempt to specify a single, secure and private group messaging
framework while allowing for multiple implementations can offer different
application feature sets. The concept for and thinking around MLS is similar to
that of TLS: it's better (for users) to have a single
cryptographically-correct, well-architected protocol for a capability when no
one is seeking to compete in the same area. MLS is defined via an architecture
and a protocol, with implementations of the latter left up to individual
vendors.

After nearly 18 months of hiatus on protocol changes (while implementers worked
to get their implementations to meet the then-current protocol drafts), draft
#14 of the [MLS
protocol](https://datatracker.ietf.org/doc/draft-ietf-mls-protocol/) was
submitted for final comment in May 3. Two Working Group interim meetings were
held in June on the issues raised by draft #14. [Drafts #15](https://datatracker.ietf.org/doc/draft-ietf-mls-protocol/) and
[#16](https://datatracker.ietf.org/doc/html/draft-ietf-mls-protocol-16.html)
were created and #16 was submitted for standards consideration. A new
architecture draft ([draft #8](https://datatracker.ietf.org/doc/html/draft-ietf-mls-architecture-08.html))
was created in June to reflect the changes adopted in the protocol during the
last quarter. It, too, was submitted in July for final review.

Last month we mentioned the [MLS Extensions
draft](https://datatracker.ietf.org/doc/draft-robert-mls-extensions/). By
agreement of the Working Group over the last six months, this document moves a
number of initially-imagined protocol “features” into hoped-for sanctioned
extensions (in order to speed the adoption of the currently-defined protocol).
In mid-June, this document was submitted officially to the Working Group for
adoption (as a work item for the WG to officially act on) and the document was
voted through at IETF114 in July.

Via these activities, and rapid progress on protocol implementations, it's
likely MLS will reach the status of Proposed Standard by mid-2023.

### Privacy Pass

With the rise of automated-agent (so-called *bots*) traffic on the Internet ([a
large percentage of it
malicious](https://www.imperva.com/company/press_releases/the-pandemic-of-the-internet-imperva-research-labs-reveals-bot-traffic-climbs-to-record-high-in-2020/)),
the content delivery network providers have needed to find ways to assurance
visitors are genuine. [CAPTCHA](https://en.wikipedia.org/wiki/CAPTCHA), in its
many forms, became popular with CDN operators despite being a "punish the good
ones" strategy. CAPTCHA also foils legitimate users who prefer less popular
browsers, those who use content filtering tools, those requiring access aids
and people using browsers built around Tor.
[Privacy Pass](https://datatracker.ietf.org/wg/privacypass/about/) can be seen
as a genuinely user-friendly approach: once a real human has proved they're a
real human, Privacy Pass leverages that proof around more of the Internet for a
longer duration of time. There are vestiges of OAuth/OAuth2 here - centralized
authorities control the issuance and verification of tokens - but the
protocol's design prohibits the operators of Privacy Pass services from
learning the underlying behavior of its users (*request unlinkability is the
preferred term*).

Like MLS, Privacy Pass has both
[architecture](https://datatracker.ietf.org/doc/draft-ietf-privacypass-architecture/)
and
[protocol](https://datatracker.ietf.org/doc/draft-ietf-privacypass-protocol/)
specifications working their way toward standardization. Privacy Pass also
specifies the
[scheme](https://datatracker.ietf.org/doc/draft-ietf-privacypass-auth-scheme/)
by which issued tokens are presented to participating services. Not
standardized: the mechanism by which issuers of Privacy Pass tokens assure
humans are humans.

In June, Apple announced Private Access Tokens - its branded version of Privacy
Pass - in the iOS 16 and macOS Ventura beta releases. Apple says they are
supporting [Privacy Pass
authentication](http://draft-ietf-privacypass-auth-scheme/)(with type 2 - blind
RSA - tokens)as defined in the [protocol
draft](https://datatracker.ietf.org/doc/draft-ietf-privacypass-protocol/). The
Apple scheme requires Apple hardware and leverages Apple's unique relationship
with its customers.  Apple device owners with the newest operating system
releases can experiment with Private Access Tokens today - CDN providers
Cloudflare and Fastly have demonstration Privacy Pass token issuers to try. See
[here](https://developer.apple.com/news/?id=huqjyh7k) and (especially)
[here](https://developer.apple.com/videos/play/wwdc2022/10077/) for a complete
description. This is, of course, a major step forward for Privacy Pass. Will
Google be forced for follow suit on behalf of Android device owners?


### Oblivious HTTP (now Oblivious HTTP Application Intermediation, OHAI)

Oblivious- the watchword that seems to have infected all of IETF. *Oblivious*
is code for blinding a service provider's view of the client's IP address
behind a pair of trusted intermediary servers. The key predecessor ideas
underlying OHAI were published in June as RFC9230 - [Oblivious DNS over
HTTP](https://datatracker.ietf.org/doc/rfc9230/), an independent submission (by
Apple and others), brought forward as *experimental*, specifically for service
requests to the Domain Name Service. Well in advance of reaching publication
status, Oblivious DNS over HTTP raised a possibility: could *all* transactional
uses of HTTP (Online Certificate Status Protocol among them, perhaps browser
telemetry) be protected in this way? New work - Oblivious HTTP Application
Intermediation, OHAI - was chartered to bring concept of *obliviousness* to
this broader arena. The tortured name, by the way, is meant to reflect that web
browsing - whose usage pattern is more interactive than transaction - is
specifically not to be covered by this protocol definition.

The Working Group's primary specification was given a much-needed refresh in
July, updating terminology and clarifying the scope and use cases for OHAI as
being *transactional*. It is believed by the Working Group - and, it seems, by
the broader IETF community, that privacy (and more specifically blinding of the
client's IP address) is being handled by the MASQUE efforts (since proxies are
required to implement obliviousness, and MASQUE is all about proxies).

The major service providers have begun to field implementations for
interoperability testing.

### Privacy Preserving Measurement (PPM)

Measurement seems a benign word - hard to imagine anyone getting worked up over
it. It's possible to characterize almost all measurements taken on Internet
traffic as being good.  But, too often, the stated requirements for measurement
hide the real and intended uses.  Many people characterize the Internet of
today as a *surveillance economy*. Consumers have tried to fight back - with
particular support in the European Union - but with modest success.  More
recently, however, state actors have begun using the surveillance economy as a
way to justify suppression of certain Internet services, particularly the most
popular media and social media outlets who are also the biggest contributors to
the mining and correlation of personal data for commercial use. With
livelihoods threatened, newer approaches to measure without surveilling were
needed. Enter *privacy-preserving measurement*.

[Differential Privacy](https://en.wikipedia.org/wiki/Differential_privacy) is
the approach being taken by IETF to address intrusive measurement.
Differential Privacy is a way to describe patterns within a dataset (say,
access to a website) without disclosing individual actors in the dataset.
However, the current state of play among the top data-harvesting firms is such
that individuals can be targeted even in the presence of early versions of this
technique. To solve these problems, *distributed aggregation* is being tried,
leveraging new mathematical techniques as well as some of the work mentioned
above (OHAI in particular). Two approaches -
[Prio](https://eprint.iacr.org/2021/576.pdf) and
[STAR](https://datatracker.ietf.org/doc/html/draft-dss-star) - are now being
considered. Based on Prio, the [Distributed Aggregation Protocol for Privacy
Preserving Measurement](https://datatracker.ietf.org/doc/draft-ietf-ppm-dap/)
was made an official working group document in May 2022. Mailing list
discussion now centers on initial interoperability testing - a step forward,
indicating more interested parties are willing to invest in implementations, at
least for study/research. This is especially pertinent because there are still
open theoretical questions about the actual privacy this solution can offer.

## Smaller standards activities

### Encrypted Client Hello (ECH) (formerly, Encrypted Server Name Identifier, ESNI)

The need for encryption of the TLS protocol handshake is described in this [excellent article](https://blog.cloudflare.com/encrypted-client-hello/) on
the Cloudflare blog:

> The most widely used cryptographic protocol for [exchanging encryption keys]
> is the [Transport Layer Security
> (TLS)](https://www.cloudflare.com/learning/ssl/transport-layer-security-tls/)
> handshake. Today, a number of privacy-sensitive parameters of the TLS
> connection are negotiated in the clear. This leaves a trove of metadata
> available to network observers, including the endpoints' identities, how they
> use the connection, and so on.

> ECH encrypts the full handshake so that this metadata is kept secret.
> Crucially, this closes a long-standing privacy leak by protecting the Server
> Name Indication (SNI) from eavesdroppers on the network. Encrypting the SNI
> is important because it is the clearest signal of which server a given client
> is communicating with. However, and perhaps more significantly, ECH also lays
> the groundwork for adding future security features and performance
> enhancements to TLS while minimizing their impact on the privacy of end
> users.

[Draft 14](https://datatracker.ietf.org/doc/draft-ietf-tls-esni/) of the
Encrypted Client Hello specification was released in February. Though not all
specification features are closed, implementation work is underway. Perhaps the
most interesting effort is [*Developing ECH for OpenSSL
(DEfO)*](https://defo.ie/). Guardian Project team members are involved in the
implementation and interoperability testing work done by this project. In
addition to making the changes in OpenSSL itself (a widely-used open source
software library implementing the TLS specification), key open source software
platforms that integrate OpenSSL - [nginx](https://nginx.org/en/),
[apache2](https://httpd.apache.org/), and [haproxy](https://www.haproxy.org/)
among them - are also being modified. This is an excellent example of the
Internet Freedom community bringing some of its core expertise (implementation
of, and experience with, encryption) to the broader open source software
community (which, in many cases, is providing dominant software that underlies
most Internet applications).

### HPKE

Since late 2018, the IETF and its companion research organizations have been
working very hard on updating the Internet's pre-existing uses of cryptography
and applying new advances to modern protocol proposals and applications. Among
the major advances, the IETF's [Hybrid Public Key Encryption
(HPKE)](https://www.rfc-editor.org/rfc/rfc9180.html) was crowned a standard -
RFC9180 - in February. Cloudflare has an [excellent
summary](https://blog.cloudflare.com/hybrid-public-key-encryption/) with
references. Many of the Internet Drafts I've cited in here depend on HPKE,
including Oblivious HTTP Application Intermediation and Messaging Layer
Security. TLS Encrypted Client Hello (ECH), above, is also dependent on HPKE.

### HTTP Transport Authentication

We mentioned that *HTTP Transport Authentication* was,
[originally](https://tools.ietf.org/id/draft-schinazi-masque-00.html) (February
2019), called MASQUE. The specification is designed to authenticate protocol
flows in a manner that does not reveal any information to an attacker.
Therefore, applications using *HTTP Transport Authentication* are resistant to
active probing by network adversaries. However, the ideas originally expressed
got IETF's creative juices flowing and, as above, a Working Group was formed to
define the standards around carrying all types of traffic (IP, UDP, and TCP)
over HTTP/3 and QUIC (and therefore leveraging QUIC's many perceived benefits:
performance, congestion control and enhanced encryption). The HTTP Transport
Authentication draft languished while this effort was in high gear. With HTTP/3
Datagrams and CONNECT-UDP now standardized and CONNECT-IP ready for
interoperability testing, it was time to dust off this draft and build an
actual implementation.

At IETF113 in March, we picked up work originally done by one of our teammates
implementing [draft #5](https://www.ietf.org/archive/id/draft-schinazi-httpbis-transport-auth-05.html)
of HTTP Transport Authentication and joined the
[Hackathon](https://github.com/IETF-Hackathon/ietf113-project-presentations/blob/main/HTTP_Transport_Auth_ietf113.pdf)
with a project to get that code working on an updated environment and in a
testable way. We got the original code running in Google Conscrypt (TLS for
Java/Android), verified its function (as defined in the Internet Draft) and
created a public open source repository with a demonstration capability. We
presented the work to Hackathon attendees (~50 people) and discussed the work
with the specification's author. Here's our implementation
[repository](https://github.com/guardianproject/HTTPTransportAuthentication).

Subsequently, Guardian Project became a co-author of an updated [draft #7](https://datatracker.ietf.org/doc/draft-schinazi-httpbis-transport-auth/07/)
of HTTP Transport Authentication which was
[presented](https://datatracker.ietf.org/meeting/114/materials/slides-114-httpbis-transport-authentication-00)
at IETF114 in July to the HTTPbis Working Group, responsible for maintenance of
and extensions to the HTTP protocol. The Working Group requested minor
modifications to the draft which will subsequently be submitted for possible
adoption as a work item. The existence of an open source implementation along
with interest from the Internet Freedom community (in the form of participation
by the Guardian Project) could be important factors in the decision to proceed
with this work.

## Opportunities ahead

The IETF is fortunate to have gifted individuals from the Internet Freedom
community deeply involved, advocating for human rights in the design of the
Internet and assuring that specifications produced adhere to high standards of
security and privacy while becoming more accessible by people in challenging
economic and cultural situations. The effort undertaken here sought to create a
two-way street between the knowledge and expertise of the Internet Freedom
community and the IETF standards community for the benefit of both. The effort
indicates that there remains ample opportunity for the technical members of the
Internet Freedom community to extend their expertise into what has become an
increasingly-important niche within IETF: real-world experience with
implementing privacy and Internet Freedom technologies in the face of
entrenched adversaries, dominant service providers and recalcitrant
governments. Efforts like Encrypted Client Hello and HTTP Transport
authentication indicate that, based on the strong relationships already
developed (and partly due to the present project), technical activities by
members of the Internet Freedom community, within the IETF framework, can more
rapidly enhance the long-term benefits of IETF's efforts for the citizens of
the Internet. Mountains from molehills, perhaps.
