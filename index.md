---
layout: single

excerpt: ""
header:
  overlay_image: /assets/images/banner.jpg
  caption: "Photo credit: [**Unsplash**](https://unsplash.com)"
#  cta_label: "More Info"
#  cta_url: "https://unsplash.com"

sidebar:
    nav: "quicklinks"

---

{% include toc icon="file-text" %}

*Welcome to the Pluggable Transports site! Looking for a place to start? Use our Getting Started guide to help you find your way around.*

<button onclick="myFunction()">Try me!</button>

<script>
function myFunction() {
    window.open("gettingstarted.html", "_blank", "toolbar=no,scrollbars=yes,resizable=no");
}
</script>

---

# Latest News

{% include feature_row_posts %}

---

<!-- **Internet censorship continues to rise** -- As reported by Freedom House's Freedom on the Net, 2017 was the [seventh consecutive year](https://freedomhouse.org/report/freedom-net/freedom-net-2017) of overall decline in internet freedom. Access Now continue to document internet shutdowns through their #KeepItOn campaign, reporting 61 shutdowns in the first three quarters of 2017.or 2016, Access Now's [#KeepItOn](https://www.accessnow.org/keepiton) campaign documented [**56 internet** shutdowns](https://www.accessnow.org/keepiton/).-->




Censorship and shutdowns vary from blocking specific websites to blocking or throttling entire types of traffic, like https or VoIP protocols. Pluggable Transports can keep sites and apps working as they are meant to when the network is being interfered with in this way.

<img src="/assets/images/comic.png" alt="What are Pluggable Transports?" />

# Using the site

If you're interested in Pluggable Transports, it's probably because you are :

- An [App Developer](/implement/) who is facing censorship
- A [Transport Developer](/build/) who knows the basics and wants to contribute
- An [End User](/about/) facing censorship, or just curious about Pluggable Transports



* **[Learn more](/how/)** - about censorship and how Transports protect traffic.

# Want to add Pluggable Transports to your tool?

There are many ways to add Pluggable Transports to your app - whether you want to proxy your traffic through a separate process or implement them as a library internal to your app.

* Visit our **[implementation](/implement/) section** to get started with mobile and desktop libraries!
* Browse the **[Transports Library](/transports/)** to get an idea of what transports you might like to start with.

# Want to build a new Transport?

Check out our section on [creating a new transport](/build/)