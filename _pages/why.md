---
layout: single
title: "Why obfuscate?"
permalink: /why/

excerpt: ""
header:
  overlay_image: /assets/images/unsplash_Francisco Casero_empty-track.jpg
  caption: "Photo credit: [**CC0 Unsplash / Francisco Casero**](https://unsplash.com)"
#  cta_label: "More Info"
#  cta_url: "https://unsplash.com"

sidebar:
    nav: "sidenav"
---

Internet censorship and surveillance are on the rise -- just between 2014 an 2015, [14 countries passed new laws](https://freedomhouse.org/report/freedom-net/freedom-net-2015) to increase surveillance and upgraded their surveillance technologies. Censorship varies from blocking specific websites or search engines to censoring or throttling specific protocols and services. Access Now's #KeepItOn campaign documented [**56 internet** shutdowns in 2016](https://www.accessnow.org/keepiton/).

{% include toc icon="file-text" %}

While often thought of as something governments do, censorship can also happen due to "parental control" tools, or be enforced by institutions or service providers to manage their networks.  

Citizens of countries with such censorship use VPNs (Virtual private network), Proxies and other software like Tor to bypass the censorship and reroute their traffic away from their country’s firewall.

## History of DPI filtering

VPN and Tor provide encrypted tunnels for the users in these countries which allow them not only bypass censorship but encrypt the traffic which prevents government from surveillance.

Targeting VPN, Tor and other service is doable by filtering the packets of such services on the Application Layer of the OSI reference model, analysis them and then block the IP addresses that clients are trying to connect to, this technology is called Deep Packet Inspection or DPI

China was one of the first countries to implement DPI against the Tor network by applying filtering against Tor’s TLS packets, this helped the Chinese Great Firewall] to identify the IP address of the Tor relays and block them immediately. [Learn more about how China blocks Tor](https://www.technologyreview.com/s/427413/how-china-blocks-the-tor-anonymity-network/
)

Other countries applied the same strategy to block Tor network like [Ethiopia](https://blog.torproject.org/blog/ethiopia-introduces-deep-packet-inspection), [Syria](http://secdev-foundation.org/wp-content/uploads/2014/08/Flash-Note-Syria-8-Syrian-regime-tightens-access-to-secure-online-communications.pdf
) [Iran](https://smallmedia.org.uk/work/writers-block-the-story-of-censorship-in-iran), and [Kazakhstan](https://bugs.torproject.org/20348#comment:184), among many others -- Privacy International tracks surveillance systems in their [Transparency Toolkit](https://sii.transparencytoolkit.org/); and Freedom House's [Freedom on the Net](https://freedomhouse.org/report/table-country-scores-fotn-2016) tracks both censorship and other limitations on net freedom.

## History of pluggable transports

Tor was able to develop a new technology that can bypass the DPI filtering system allowing Tor users to access the Internet via Tor network, they called the technology Pluggable Transports (PT).

PT is a traffic flow that Tor used to give the traffic of Tor an “*innocent-looking transformed traffic instead of the actual Tor traffic*” 

In February 2012, Iran began filtering encrypted internet traffic, triggering The Tor Project to release their first public [obfuscation work](https://blog.torproject.org/blog/obfsproxy-next-step-censorship-arms-race).

<!-- MORE -->

## Filtering Internet Traffic

Advanced firewalls can classify Internet's traffic based on different elements like the basic header's information of the packets which identify the port, protocol, source, destination for example.

On the other hands there is more in depth classification which classify traffic based on the actual data that packets are carrying.

<!-- MORE -->
