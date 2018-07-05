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

*Welcome to the Pluggable Transports website -- We'd love your feedback via [email](mailto:feedback@pluggabletransports.info) or our [issue queue](https://github.com/OpenInternet/PT-website/issues). If you want to read or contribute to the the spec, you can do that [here](https://github.com/Pluggable-Transports/Pluggable-Transports-spec).*

**Internet censorship continues to rise** -- As reported by Freedom House's Freedom on the Net, 2017 was the [seventh consecutive year](https://freedomhouse.org/report/freedom-net/freedom-net-2017) of overall decline in internet freedom. Access Now continue to document internet shutdowns through their #KeepItOn campaign, reporting 61 shutdowns in the first three quarters of 2017.or 2016, Access Now's [#KeepItOn](https://www.accessnow.org/keepiton) campaign documented [**56 internet** shutdowns](https://www.accessnow.org/keepiton/).

Censorship and shutdowns vary from blocking specific websites to blocking or throttling entire types of traffic, like https or VOIP protocols. Pluggable Transports can keep sites and apps working as they are meant to when the network is being interfered with in this way.

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