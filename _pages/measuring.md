---
layout: single
title: "Measuring Censorship"
permalink: /measuring/

excerpt: ""
header:
  overlay_image: /assets/images/max-vertsanov-683738-unsplash.jpg
  caption: "Photo credit: [**CC0 Unsplash / Max Vertsanov**](https://unsplash.com/@make_it)"
#  cta_label: "More Info"
#  cta_url: "https://unsplash.com"

sidebar:
    nav: "aboutnav"
---

Censorship of Internet content is increasing, as more regimes seek to restrict the flow of information to and between their citizens. Pluggable Transports can help keep people connected, by hiding (obfuscating) network traffic and routing it between devices in ways that make it more difficult to block.

You can see from our [overview](/how) that there are several methods censors can use to obstruct the Internet, and how there are different ways people can get past these restrictions. But how do we know what's really happening around the world, and how do we decide where our attention is needed the most?

Below is a collection of resources that may help you find out some of this information. These are projects that do ongoing monitoring of network outages, and report them to the [community](/community). If you know of a resource we should be including here, let us know via [GitHub issues](https://github.com/Pluggable-Transports/Pluggable-Transports-website/issues)

* **[OONI](https://explorer.ooni.torproject.org)** - The Open Observatory of Network Interference is part of the [Tor Project](https://www.torproject.org), aiming to collect information related to network interference around the world. They have been operational for over 5 years, and have [a collection of apps](https://ooni.torproject.org/install/) run by volunteers to test network conditions where they are.<br /><br />OONI's raw data is often combined with research from leading academics and practitioners to build up a picture of censorship at specific times, such as [this example from August 2018](https://ooni.torproject.org/post/south-sudan-censorship/) that provides some detail about South Sudan's blocking of independent news organizations.

* **[Censored Planet](https://censoredplanet.org)** - The Censored Planet Observatory uses remote measurement techniques to run remote censorship measurements for thousands of websites. Censored Planet continously measures reachability to 2,000 websites from more than 95,000 vantage points in 221 countries (a 42-360% increase compared to other measurement platforms). Recently Censored Planet has partnered with Google to create [Jigsaw](https://jigsaw.google.com/), which addresses open society threats by developing scalable tech solutions, focusing on areas like disinformation, censorship, online toxicity, and violent extremism.

* **[IODA](https://ioda.inetintel.cc.gatech.edu/)** - IODA stands for Internet Outage Detection and Analysis, a project from CAIDA, the Center for Applied Internet Data Analysis. IODA's visualizations are considered "experimental", and the site urges people to use it with caution. Along with a high-level [dashboard](https://ioda.inetintel.cc.gatech.edu/) showing outages according to their severity, IODA also has an [Alert Feed](https://ioda.inetintel.cc.gatech.edu/) for outages, and a comprehensive [API](https://ioda.inetintel.cc.gatech.edu/) for researchers to use with their own dta.

* **[MLab](https://measurementlab.net)** - The Measurement Lab is an open source project, hosting [tests](https://www.measurementlab.net/tests/) that individual users can access to verify their own connections. The data is collected according to a strict [Privacy Policy](https://www.measurementlab.net/privacy), and the project publishes all data in an open format, as well as providing its own [visualizations](https://www.measurementlab.net/visualizations/) and [publications](https://www.measurementlab.net/publications/)

* **[RIPE NCC](https://stat.ripe.net)** - Operating at the routing level of the Internet, RIPE measures the performance of the Internet's backbone. Using data from [RIPE Atlas devices](https://atlas.ripe.net/) used by thousands of organizations around the world, RIPE able to provide a snapshot of Internet performance from a number of different perspectives.

* **[Canary](https://github.com/OperatorFoundation/Canary)** - Operator Foundation's Canary platform is a server-based tool for assessing the efficacy of pluggable transports, working in conjunction with their [Adversary Lab](https://github.com/OperatorFoundation/AdversaryLabClientSwift) client. As a privately-run tool, tests can be run on all pluggable transports that work with Operator's [Shapeshifter](https://github.com/OperatorFoundation/shapeshifter-dispatcher) library.

* **[Psiphon Data Engine](https://psix.ca)** - Launched in 2020, the Psiphon Data Engine \(PDE\) is a central repository for visualizing and analyzing open network measurements, with the aim of providing actionable insights for researchers and security trainers. \(PDE\) has a number of [partners](https://psix.ca/d/IvwsRueWz/partners?orgId=2) and is actively soliciting for more engagement from across the Internet Freedom community.

* **[Tor Metrics](https://metrics.torproject.org)** - Tor already has support for Pluggable Transports, deployed through obfuscated bridges. Their metrics page can give a good indication of where network blocking takes place. On the metrics page, you can see where direct connections are working, and also where different types of bridge connections are taking place. This data is supplemented by a manual collection of known events that might be related to the graphs. Also worth looking at is Torsten Grote's visualization of data collected by OONI, and showing where [Tor may be being blocked](https://grobox.de/tor/).

* **[Google Transparency Report](https://transparencyreport.google.com/traffic/overview)** - Originally launched as far back as 2010 during Google's conference [Internet at Liberty]( https://sites.google.com/a/pressatgoogle.com/internet-at-liberty-2010/), the Transparency Report documents a number of ways in which Google's services are restricted internationally. Sometimes that can be via [government demands for content takedown](https://transparencyreport.google.com/government-removals/overview), but other times it can be due to interference at the network level. The [Global Disruptions](https://transparencyreport.google.com/traffic/overview) map shows a near-live, aggregated view of what they are seeing on their network, along with links to verified reports that the data illustrates.

* **[Netblocks](https://netblocks.org)** - Netblocks operate at the intersection of cybersecurity, Internet governance, and digital rights. The organization was originally formed as [Turkeyblocks](https://www.turkeyblocks.org) when Turkey blocked access to social media in the wake of a bombing attack in Ankara. They have since moved on to test conditions in many other countries, collecting data via a network of volunteers in countries such as Iraq, Nicaragua and Pakistan. In August 2018, they wrote about the restriction of mobile Internet speeds in [Bangladesh](https://netblocks.org/reports/bangladesh-internet-shutdown-student-protests-jDA37KAW), and at the time of writing are due to launch COST, their collaboration with the [Internet Society](https://www.internetsociety.org/) to measure the cost of shutting down the Internet, at the [2018 Forum on Internet Freedom in Africa](https://cipesa.org/2018/06/2018-edition-of-the-forum-on-internet-freedom-in-africa-fifafrica-set-to-take-place-in-ghana/).


* **[Freedom on the Net](https://freedomhouse.org/report/freedom-net/)** - If you're looking at how Internet Freedom has been impacted over the past since 2012, you can read through the archive of reports produced by Freedom House. Using networks of trusted reporters, Freedom House have amassed a wealth of information and detailed reports, showing a year-on-year decline of overall freedom on the net. For up-to-date reports since the [latest edition](https://freedomhouse.org/report/freedom-net), you can also follow them on [Twitter](https://twitter.com/freedomonthenet).

* **[Internet Society (ISOC)](https://pulse.internetsociety.org/)** - The Internet Society's Pulse platform provides data and insights on the health and availability of the Internet in various regions. It includes information on Internet shutdowns, disruptions, and threats to the Internet's core infrastructure.

* **[Ranking Digital Rights (RDR)](https://rankingdigitalrights.org/)** - evaluates and ranks the world's most significant digital platforms and telecommunications companies on their policies and practices related to freedom of expression and privacy. Their annual Corporate Accountability Index provides valuable insights into how these companies' practices affect users' rights. 

* **[Accessnow](https://www.accessnow.org/campaign/keepiton/)** - defends and extends the digital rights of people and communities at risk. This takes the form of helplines, holding summits, providing grants, advocating, and operations, including the KeepItOn campaign which documents internet outages and their various impacts on communities. .

* **[Mozilla Network Outages Data Project](https://wiki.mozilla.org/Mozilla_Network_Outages_Data_Project#:~:text=Mozilla%20offers%20public%20access%20to,and%20shutdowns%20around%20the%20world.)** - This initiative focuses on creating an anonymized, privacy-preserving dataset of signals derived from existing Mozilla telemetry, which correlates with network outages in various countries and cities worldwide. To ensure the reliability of these outage signals, the project involves validation by both internal network experts and esteemed external specialists. It is committed to disseminating its findings and making the dataset publicly available, contributing to broader knowledge and understanding of internet health issues.

* **[Cloudflare Radar](https://blog.cloudflare.com/introducing-cloudflare-radar/)** - The goal is to help build a better Internet by exposing insights, threats and trends based on aggregated data tracked by Cloudflare. In order to understand what is happening on the Internet from a security, performance and usage perspective. As well as partnering with [civil society](https://blog.cloudflare.com/partnering-with-civil-society-to-track-shutdowns) to provide tools like Radar and Internet shutdown alerts. Every Internet user should have easy access to answer the questions that they have.

*  **[Internet Borders: Hackathon Events](https://internetborders.net/)** - Ongoing conferences that seek to address critical digital issues such as online censorship, propaganda, and network outages.Through collaborative events, they bring together experts and enthusiasts to delve into challenges like analyzing censorship mechanisms, investigating network disruptions, and scrutinizing propaganda tactics. These hackathons not only foster innovation and research in these areas but also offer participants the opportunity to contribute meaningful insights and solutions, with the potential for their work to be recognized and rewarded.
 

<p style="text-align:left;"><a href="/about/">&lt; Previous</a>
</p>
