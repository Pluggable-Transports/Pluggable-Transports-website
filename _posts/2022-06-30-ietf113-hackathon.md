---
title: "IETF 113 Hackathon"
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

## IETF113 Hackathon Review

By David Oliver of Guardian Project -
[david@guardianproject.info](mailto:david@guardianproject.info), Hans-Christoph
Steiner of Guardian Project

**TL;DR**

- [Specification](https://www.ietf.org/archive/id/draft-schinazi-httpbis-transport-auth-05.html)

- [Implementation Repo](https://github.com/guardianproject/HTTPTransportAuthentication)

We picked up the work originally done by teammate Ali Hussain, got it running
in newest Google Conscrypt (TLS for Java/Android), verified its function and
created a public open source repository. We presented the work to Hackathon
attendees (~50 people), took questions from the general IETF community, and
discussed the work with the specification author.

### Clarification

When we initiated our work in this area, it was called MASQUE (Multiplexed
Application Substrate over QUIC Encryption). The [initial
draft](https://tools.ietf.org/id/draft-schinazi-masque-01.html) discussed the
mechanism we describe here, but also addressed some wider ideas. The draft
generated a lot of interest in the IETF Transport Area, a Working Group was
formed and a broad set of activities - all use cases for proxying over QUIC -
was taken on by that group. The working group decided to excise the specific
VPN use case – the one we’re most interested in – using a slightly different
proposal to be added to HTTP as an extension. That extension is called *HTTP
Transport Authentication*.

### Why HTTP Transport Authentication?

[HTTP Transport Authentication](https://www.ietf.org/archive/id/draft-schinazi-httpbis-transport-auth-05.html)
was proposed with modern HTTP use cases in mind - specifically those that don’t
follow the classic request/response model. WebTransport and MASQUE are examples
of protocols that leverage the advantages of HTTP but open durable and
sometimes interactive protocol flows with other servers. *HTTP Transport
Authentication* is designed to authenticate such protocol flows in a manner
that does not reveal any information to an attacker during failure cases.
Therefore, applications using *HTTP Transport Authentication* are resistant to
active probing by network adversaries. The specification describes two types of
authentication scheme (already common), but also supports extensions for
situations where novel authentication schemes are useful or required.

### How does HTTP Transport Authentication Work?

*HTTP Transport Authentication* is defined as a new HTTP [Request
Header](https://datatracker.ietf.org/doc/html/rfc7231#section-5) valid for all
versions of HTTP. In HTTP/1.1, it is designed to accompany the
[CONNECT](https://datatracker.ietf.org/doc/html/rfc7231#section-4.3.6) method.
The client request arrives at the web server via HTTPS (HTTP over TLS) and,
thus, prior to HTTP protocol execution, an encrypted TLS session is available,
over which protocol messages flow. *HTTP Transport Authentication* uses
[RFC5705 Keying Material Exporters](https://www.rfc-editor.org/rfc/rfc5705) to
extract a derivative encryption key from the TLS session, and uses the key to
symmetrically encrypt an authentication token presented with the CONNECT verb
in the form of a new HTTP header:

`CONNECT server.example.com:443 HTTP/1.1`\
`Host: server.example.com:443`\
`Transport-Authorization: Signature a=AVAL; u=UVAL; p=PVAL(encrypted)`

The web server receives this request, uses the same technique to extract the
same derivative key from the TLS session and uses the key to decrypt the P
value in the *Transport-Authorization* header. If the decrypted value - along
with the other information provided- matches an entry on the server side, the
server can be satisfied that this client is permitted to use a [proxied
connection through
it](https://datatracker.ietf.org/doc/html/rfc7231#section-4.3.6) to a
destination origin server. Failure cases - lack of a Transport-Authentication
header, improper encryption of the header, improper authentication information
or the request otherwise badly-formed - result in a generic error response
([405 Method Not
Allowed](https://datatracker.ietf.org/doc/html/rfc7231#section-6.5.5)). Network
entities actively probing the web server with badly-formed requests will also
receive this generic response - the standard response for web servers that
don’t offer CONNECT/proxying at all, making the host offering the service
“invisible” in all cases except the success case.

## Implementation Details

The popular (and nearly ubiquitous) OpenSSL library - along with its Google
counterpart, BoringSSL - implements the function
`SSL_Export_Keying_Materials()` which itself implements the RFC5705 standard.
Many higher-level programming libraries and languages bind to one of these
implementations for their SSL/TLS support. Java (OpenJDK, Conscrypt, Cronet),
Go and Rust are examples. PyOpenSSL for Python binds to OpenSSL, as does
Python’s native SSL support, though the latter does not support this specific
method. Microsoft’s NSS library supports the function.

`SSL_Export_Keying_Materials()` takes a string as input (officially, an
[IANA-defined
entry](https://www.ietf.org/archive/id/draft-schinazi-httpbis-transport-auth-05.html#section-7.2))
that it uses to create a derived key from the underlying TLS connection’s
master secret. This key, unique to the session, is used by the client to
encrypt an authentication token placed in a new HTTP header which the server
can decrypt with the same key derived in the same way from its underlying
connection. The server then matches that authentication token(s) provided
against its records before returning connection status.

## Implementation

Based on earlier work, we wrote an implementation of the new header and its
handling without a complete proxied flow. In other words, the client will
attempt to CONNECT using this technique, the web server will process the
request (yielding failure or success) and immediately close the client’s
session. Our demonstration is written in Java (Conscrypt Java for Google
Android). Conscrypt itself is unchanged.

### Possible Next Steps

An improved - more complete - demonstration would be beneficial. We do not have
the funding under this project to complete this work, however. We will explore
other opportunities in the Internet Freedom space. Should funding become
available, a logical next step is to implement the new function in Python, in
an existing open source proxy server. Two are easily available:

1. [Proxy2 (Python)](https://github.com/inaz2/proxy2), small codebase, simple
   function:
   - First step would be to get this working with PyOpenSSL (to get the
     keying material function)
   - Second step would be to add the HTTP TA function
2. [Python Static and Reverse Proxy](https://github.com/swinkelhofer/python_proxy),
   slightly richer codebase (reverse proxy functions, too):
   - Same steps as above

### Possible Consumers of this Capability

Viewed optimistically given its resistance to probing attacks, *HTTP Transport
Authentication* potentially turns every web server into a VPN tunnel in the
same way *Encrypted Client Hello* potentially turns every web server into a
domain front. Many parties implementing Pluggable Transports - VPN service
providers, for example - have likely developed special client verification
handshakes allowing only designated users and software access to their service
infrastructure. *HTTP Transport Authentication* might offer a standard and more
probe-resistant alternative that better “blends in” with standard web server
functions. Alternatively, the client and bridge components of the pluggable
transport itself could implement *HTTP Transport Authentication* “privately”
between them (using, for example, a private authentication scheme).

### Discussion with the Author of the Internet Draft

As expected, the author noted that this work has been sidelined mostly for
personal reasons (too much other IETF responsibility including the MASQUE
Working Group, the WebTransport Working Group and the Internet Architecture
Board), but also because the definition of MASQUE has been fluid for two years
and the requirements of those protocol changes have not been fully understood.
As the MASQUE work comes to completion (2022), *HTTP Transport Authentication*
might see the light of day again in some modified form, to be decided. In the
meantime, the proposal stands as a published and available draft. Our work
provides a demonstration code base implementing it.
