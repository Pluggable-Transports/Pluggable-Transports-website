---
layout: single
author_profile: false

title: "Mobile VPN with Pluggable Transports"
permalink: /implement/mobilevpn

excerpt: ""
header:
  title: "Mobile VPN with Pluggable Transports"
  overlay_image: /assets/images/unsplash_0wdpet-ufqs-todd-diemer.jpg
  caption: "Photo credit: [**CC0 Unsplash / Todd Diemer**](https://unsplash.com/@todd_diemer)"
#  cta_label: "More Info"
#  cta_url: "https://unsplash.com"
sidebar:
    nav: "impnav"
---

# Installing Bitmask mobile VPN with support for Pluggable Transports #

The Pluggable Transports team worked with [Calyx](https://calyx.net) to add Pluggable Transports support to their product, which is based on the [Bitmask](https://bitmask.net) VPN. As of March 2024, the [Bitmask](https://bitmask.net) VPN is available for android devices through Google Play download, and for desktop clients ([Linux](https://bitmask.net/en/install/linux)/Windows/MacOS) it’s available by installing RiseupVPN.

For android devices unable to install Bitmask VPN from the Google Play Store, the video on this page guides you through the steps of installing [F-Droid](https://f-droid.org), downloading and installing the app, and configuring it to use Pluggable Transports.


<iframe width="560" height="315" src="https://www.youtube.com/embed/RxVL3bpCQSg" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>


# Steps Taken #
1. Download and install F-Droid from [https://f-droid.org](https://f-droid.org)
2. Open F-Droid and the catalog will update.
3. Use the green Search button to find Bitmask VPN.
4. Install Bitmask VPN. When asked, choose to "Allow from this source".
5. Open Bitmask, and choose the provider "riseup.net".
6. Select OK to approve the VPN connection setup request.
7. Select the option "Using Bridges" to enable Pluggable Transports.
8. Press the key icon to start the VPN.
9. When connected, you will see a key in the status bar at the top of the screen.
10. When finished, press the big key button to switch the VPN off.
