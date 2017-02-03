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
{% include toc icon="file-text" %}

*Welcome to the new Pluggable Transports website -- We'd love your feedback via [email](mailto:feedback@pluggabletransports.info) or our [issue queue](https://github.com/OpenInternet/PT-website/issues)*

**Internet censorship is on the rise** -- just between 2014 and 2015, [14 countries passed new laws](https://freedomhouse.org/report/freedom-net/freedom-net-2015) to increase control over internet traffic. For 2016, Access Now's #KeepItOn campaign documented [**56 internet** shutdowns](https://www.accessnow.org/keepiton/).

Censorship and shutdowns vary from blocking specific websites to blocking or throttling entire types of traffic, like https or VOIP protocols.

# What are Pluggable Transports?

Let's break that apart.

**Transports** are an [ever-growing collection](/transports/) of tools which follow a single [specification](/spec/) which any application can adopt and defeat censorship by making its network traffic almost impossible to identify and block. Some do this by scrambling the traffic so that it's indistinguishable, some disguise the traffic to look like it's just hitting a popular cloud service, and others make the traffic look like "allowed" traffic.  

**Pluggable** refers to the fact that each transport can be swapped in and out - so if one approach is blocked, an app using Pluggable Transports could switch to another method and be back online immediately.  An adversary would have to be willing and able to block all forms of transports at once to truly block your application.

With censorship of tools and websites increasingly common -- and powerful -- this is an urgent need to ensure access to information.

* **[Learn more](/how/)** - about censorship and how Transports protect traffic.


# Want to add Pluggable Transports to your tool?

There are many ways to add Pluggable Transports to your app - whether you want to proxy your traffic through a separate process or implement them as a library internal to your app.

* Visit our **[implementation](/implement/) section** to get started with mobile and desktop libraries!
* Browse the **[Transports Library](/transports/)** to get an idea of what transports you might like to start with.

# Want to build a new Transport?

Check out our section on [creating a new transport](/build/)