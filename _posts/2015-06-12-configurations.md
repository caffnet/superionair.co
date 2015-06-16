---
layout: post
title: Configuring our Defense
subtitle: "Getting ready to fly"
cover_image: dc3-f-logo.png
excerpt: "Superion Air took a real hard look at how exactly they would defend against their bot problem."
author:
  name: John Stauffacher
  twitter: g33kspeed
  gplus: 115459133183253104599
  bio: Defensive Security Expert
  image: js.jpg
---
<span class="firstLetter">T</span>he staff at Superion Air worked diligently to configure their new found devices for the environment. During this period of time they reached out to all the manufacturers to get a configuration set that would match their environment. For the purposes of these tests, below are the settings that were used. 

##F5 ASM
<span class="firstLetter">T</span>he F5 ASM was configured with a standard virtual server with an Application Security profile and DDOS Mitigation profile applied. The main ASM signatures were left default. No extra signatures were loaded, as the staff only wanted to use the Web Scraping, Anti Bot, and DDOS profiles. 
* The Virtual Server config
{% image f5_asm_vs_config.png %}
* The setup screen
{% image f5_asm_setup1.png %}
* The signature set
{% image f5_asm_signaturesets_loaded.png %}
* The F5 ASM Policy
{% image F5_asm_policy.png %}

<span class="firstLetter">T</span>he F5 ASM was configured with a login page setting, as well as Brute Force mitigation settings so the device can track brute force logins: 
* The F5 ASM Login Page Configuration
{% image f5_asm_login_page.png %}

<span class="firstLetter">T</span>he F5 ASM was also configured with Anti-DDOS settings
* {% image f5_anti_ddos_settings.png %}

<span class="firstLetter">T</span>he staff at Superion Air was also having some issues with a competitor that had hired out to companies in Brazil and Singapore to launch attacks against them. The wanted to make sure that any client connections from those two countries were blocked.
* {% image f5_asm_geolocation.png %}

<span class="firstLetter">F</span>inally the staff at Superion Air configured the Anti Bot settings of the F5 ASM.
* {% image f5_bot_detection.png %}
* {% image f5_bot_detection2.png %}
* {% image f5_bot_detection3.png %}
* {% image f5_bot_detection4.png %}

##Distil Networks
<span class="firstLetter">T</span>he Distil Networks appliance was configured similar to the F5 ASM. The configuration is outlined below:
* {% image distil_configuration_1.png %}

<span class="firstLetter">C</span>ontent Protection was then configured
* {% image distil_configuration_content.png %}

<span class="firstLetter">T</span>he Custom Pages, and IP Access list were left untouched as the staff felt there was no need to add anything to these configurations. The Country Block List was then configured
* {% image distil_geofence.png %}

<span class="firstLetter">T</span>he Content Distribution section was configured to _NOT_ cache any content on the Distil Appliance. Just like the F5 ASM configuration, the device will not be caching any content to make our testing easier.

* {% image distil_content_cache %}



