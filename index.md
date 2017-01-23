---
layout: single

excerpt: ""
header:
  overlay_image: /assets/images/banner.jpg
  caption: "Photo credit: [**Unsplash**](https://unsplash.com)"
#  cta_label: "More Info"
#  cta_url: "https://unsplash.com"

sidebar:
    nav: "sidenav"
---

Pluggable Transports are a concept developed by [The Tor Project](https://www.torproject.org/docs/pluggable-transports.html.en) that provide ways for tool developers to make their apps more resilient and keep users connected to online services despite attempts to shut them down.

With censorship of tools and websites increasingly common -- and powerful -- this is an urgent need to ensure access to information.

* [Why?](/why/) - learn more about current censorship approaches and trends

* [How?](/how/) - learn how Transports protect traffic against blockage.


# Want to add Pluggable Transports to your tool?

There are many ways to add Pluggable Transports to your app - whether you want to proxy your traffic through a separate process or implement obfuscation as a library internal to your app.



[OpenVPN Walkthrough](/implement/openvpn/)

# Want to build a new Transport?

First, it's worth perusing the [Transports Library](transports) to see current transports and approaches to the problem.  Specifically, reading through the evaluations of popular transports will help understand and identify common challenges. You can read the [Evaluation Criteria](https://trac.torproject.org/projects/tor/wiki/doc/PluggableTransports/PTEvaluationCriteria) used as well as the [expectations for inclusion into the Tor Browser Bundle](https://trac.torproject.org/projects/tor/wiki/doc/PluggableTransports/GuidelinesForDeployingPTs).  Tor's [Pluggable Transport Documentation](https://trac.torproject.org/projects/tor/wiki/doc/PluggableTransports) contains years of compiled wisdom.

