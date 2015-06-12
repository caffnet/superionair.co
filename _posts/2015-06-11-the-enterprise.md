---
layout: post
title: The Enterprise
subtitle: "So what's under the hood at Superion Air"
cover_image: dc3-i-logo.png
excerpt: "Let's take a peak under the hood at Superion Air"
author:
  name: John Stauffacher
  twitter: g33kspeed
  gplus: 115459133183253104599
  bio: Defensive Security Expert
  image: js.jpg
---
<span class="firstLetter">T</span>he staff at Superion Air have done quite a bit with not a lot for some time. Their main infrastructure features a cloud enabled application, and a database server. All of these run on a cluster of VMWare ESX 5.5 servers. These servers are aging Dell 2950 servers with dual quad core processors. Two of the machines have 16GB of ram the other has 32GB. Their storage is provided by a commodity iSCSI array.

## The Web Application
<span class="firstLetter">T</span>he key piece of their infrastructure is their website. This website runs under the WordPress CMS platform, and is the main interface between Superion Air and their customers. This Website is where Superion Air books travel for their customers, and updates their pilot schedules and maintenance operations. This website is key to Superion Air, and their current Cloud expense is making management think that they may have to start raising prices. They need to take down the bot traffic to get their Cloud expenses under control. A wpscan report is listed below and available here:
    [+] URL: https://client.superionair.co/
    [+] Started: Fri Jun 05 12:51:58 2015

    [+] robots.txt available under: 'https://client.superionair.co/robots.txt'
    [+] Interesting header: LINK: <https://client.superionair.co/>; rel=shortlink
    [+] Interesting header: SERVER: nginx/1.8.0
    [+] Interesting header: X-POWERED-BY: PHP/5.4.16
    [+] XML-RPC Interface available under: https://client.superionair.co/xmlrpc.php

    [+] WordPress version 4.2.2 identified from meta generator

    [+] WordPress theme in use: theme1770

    [+] Name: theme1770
     |  Location: https://client.superionair.co/wp-content/themes/theme1770/
     |  Style URL: https://client.superionair.co/wp-content/themes/theme1770/style.css
     |  Theme Name: theme1770
     |  Theme URI: https://superionair.co
     |  Description: Superion Air
     |  Author: Superion Air

    [+] Enumerating plugins ...
    [+] We found 7 plugins:

    [+] Name: akismet - v3.1.2
     |  Location: https://client.superionair.co/wp-content/plugins/akismet/
     |  Readme: https://client.superionair.co/wp-content/plugins/akismet/readme.txt

    [+] Name: booking
     |  Location: https://client.superionair.co/wp-content/plugins/booking/

    [+] Name: cherry-plugin
     |  Location: https://client.superionair.co/wp-content/plugins/cherry-plugin/

    [+] Name: contact-form-7 - v4.2
     |  Location: https://client.superionair.co/wp-content/plugins/contact-form-7/
     |  Readme: https://client.superionair.co/wp-content/plugins/contact-form-7/readme.txt

    [+] Name: motopress-content-editor
     |  Location: https://client.superionair.co/wp-content/plugins/motopress-content-editor/

    [+] Name: revslider
     |  Location: https://client.superionair.co/wp-content/plugins/revslider/

    [+] Name: wordpress-importer - v0.6.1
     |  Location: https://client.superionair.co/wp-content/plugins/wordpress-importer/
     |  Readme: https://client.superionair.co/wp-content/plugins/wordpress-importer/readme.txt

    [+] Enumerating installed themes  ...
    [+] We found 2 themes:

    [+] Name: slide
     |  Location: https://client.superionair.co/wp-content/themes/slide/
     |  Style URL: https://client.superionair.co/wp-content/themes/slide/style.css
     |  Description: 

    [+] Name: theme1770
     |  Location: https://client.superionair.co/wp-content/themes/theme1770/
     |  Style URL: https://client.superionair.co/wp-content/themes/theme1770/style.css
     |  Theme Name: theme1770
     |  Theme URI: https://superionair.co
     |  Description: Superion Air
     |  Author: Superion Air

    [+] Finished: Fri Jun 05 12:56:17 2015
    [+] Requests Done: 2022
    [+] Memory used: 38.754 MB
    [+] Elapsed time: 00:04:19


