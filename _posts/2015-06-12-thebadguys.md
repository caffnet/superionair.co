---
layout: post
title: The Bad Guys
subtitle: "Simulating our Attackers"
cover_image: dc3-e-logo.png
excerpt: "In the interest of a fair and balanced test. A comprehensive network of tools was enplored to simulate some very interesting test scenarios"
author:
  name: John Stauffacher
  twitter: g33kspeed
  gplus: 115459133183253104599
  bio: Defensive Security Expert
  image: js.jpg
---
<span class="firstLetter">T</span>he staff at Superion Air works hard day and day out to keep their company flying high. We decided to dip into our bag of tricks and come up with some formidable badguys that will put these devices through their places. Each one of these products and services were hand picked to fulfill a specific use case. 

##Scripts
<span class="firstLetter">S</span>cripts represent the simplist of Bots. 

|Script                   |Info                                                                 |URL                                                      |
|-------------------------|---------------------------------------------------------------------|---------------------------------------------------------|
| Perl - LWP              | Custom script written in Perl using the LWP backend                 | [{% image perl-logo.jpg %}](www.perl.org)               |
| Perl - WWW::Mechanize &nbsp; &nbsp; &nbsp;  | Custom script written in Perl using the WWW::Mechanize backend      | [{% image perl-logo.jpg %}](www.perl.org)               |
| PhantomJS               | Custom script written in JavaScript utilizing the PhantomJS library | [{% image phantomjs-logo.png %}](http://phantomjs.org/) |
| Curl                    | Custom script written with the Curl utility                         | [{% image curl-logo.jpg %}](http://curl.haxx.se/)       |

###Sample PhantomJS Script
```javascript
    var system = require('system');
    var args = system.args;
    var page = require('webpage').create();
    var url = system.args[1];
    page.open(url, function(status) {
       if(status === "success") {
          console.log(page.content);
       }
       phantom.exit();
    });
```

##Services
<span class="firstLetter">S</span>ervices represent a more sophisticated Bot infrastructure. With the exception of CyberGhost the services below were used to execute complex scripts as well as provide load testing and DDOS capabilities. Cyber Ghost was used to provide a method to source traffic from different countries and analyze the GeoFence capabilities of the products.


|Service                  |Info                              |URL                                                                            |
|-------------------------|----------------------------------|-------------------------------------------------------------------------------|
| LoadImpact              | Commercial load testing service. | [{% image loadimpact-logo.png %}](http://www.loadimpact.com)                  | 
| vBooter                 | Commercial load testing service. | [{% image vbooter-logo.png %}](https://vbooter.org)                           | 
| Bees With Machine Guns  | Open Source Load Testing Tool    | [{% image bees-logo.jpg %}](https://github.com/newsapps/beeswithmachineguns)  |
| Cyber Ghost             | Commercial VPN Provider          | [{% image cyberghost-logo.png %}](http://www.cyberghostvpn.com/en_us)         |

###Test Case Scripts
* <span class="firstLetter">S</span>cenario #1: Extreme load
```
    http.page_start("Page 1")
    local response = http.request_batch({
        {"GET", "https://$HOST"},
        {"GET", "https://$HOST/wp-includes/wlwmanifest.xml"},
    })

    log.info('Response: ' .. response[1].body)
    result.custom_metric("time_to_first_byte", response[1].time_to_first_byte)
    http.page_end("Page 1")

    client.sleep(math.random(20, 40))
```
* <span class="firstLetter">S</span>cenario #2: Failed Login Attempts
```   
         local users = { 
             { username = 'joe', password = 'secret' }, 
             { username = 'bill', password = 'secret2' }, 
             { username = 'anne', password = 'verysecret' }, 
             { username = 'jim', password = 'topsecret' }, 
             { username = 'sally', password = 'ohsosecret' }, 
         { username = 'sally2', password = 'ohsosecret' },      
      { username = 'sally3', password = 'ohsosecret' }, 
         } 
         
         -- Randomize a number which will determine which user we logon as 
         local user_number = math.random(1, #users) 
         local username = users[user_number]['username'] 
         local password = users[user_number]['password'] 
         
         -- Create POST data to log us on with this particular username/password 
         local post_data = "log=" .. username .. "&pwd=" .. password 
         
         -- Execute HTTP POST operation to log in our user 
         local response = http.request_batch({ 
             {'POST', 'https://$HOST/wp-login.php', data=post_data, response_body_bytes=10240} 
         }) 
         
         -- Extract HTTP response body 
         local response_body = response[1]['body'] 
         
         -- If we didn't get e.g. "Welcome, jim", our login failed 
         local search_for = "Welcome, " .. username 
         if string.find(response_body, search_for) == nil then 
             log.info('Failed to logon as user: ' .. username .. '--' .. response_body) 
         end
 ```

* <span class="firstLetter">S</span>cenario #3: Flight Searching 
```    
    http.request_batch({
    	{"GET", "https://$HOST/wp-content/themes/theme1770/images/datepicker_img/ui-bg_glass_100_f6f6f6_1x400.png", auto_decompress=true}
    })

    http.request_batch({
    	{"GET", "https://$HOST/wp-content/themes/theme1770/images/datepicker_img/ui-icons_ffffff_256x240.png", auto_decompress=true},
    	{"GET", "https://$HOST/wp-content/themes/theme1770/images/datepicker_img/ui-icons_ef8c08_256x240.png", auto_decompress=true}
    })

    http.request_batch({
    	{"GET", "https://$HOST/wp-content/themes/theme1770/images/datepicker_img/ui-bg_glass_65_ffffff_1x400.png", auto_decompress=true}
    })

    http.request_batch({
    	{"GET", "https://$HOST/wp-content/themes/theme1770/images/jspDrag.jpg", auto_decompress=true}
    })

    http.page_start("Page 1")
    http.request_batch({
    	{"POST", "https://$HOST/booking/?switch=Round%20Trip&from=ONT&to=BOS&dateFrom=06/16/2015%2006:05&dateTo=06/30/2015%2003:05&adult=1&child=2&infant=0", headers={["Content-Type"]="application/x-www-form-urlencoded"}, data="Adult=1&Child=2&Depart%3A=06%2F16%2F2015%2006%3A05&From%3A=ONT&Infant=NULL&Return%3A=06%2F30%2F2015%2003%3A05&To%3A=BOS&dateFormat=mm%2Fdd%2Fyy&switch=Round%20Trip&timepicker1=true&timepicker2=true", auto_decompress=true}
    })

    http.request_batch({
    	{"GET", "https://$HOST/ga941638.js", auto_decompress=true},
    	{"GET", "https://$HOST/wp-includes/js/wp-emoji-release.min.js?ver=4.2.2", auto_decompress=true},
    	{"GET", "https://$HOST/wp-content/themes/theme1770/images/border-bot.gif", auto_decompress=true},
    	{"GET", "https://$HOST/wp-content/themes/theme1770/images/marker-2.png", auto_decompress=true},
    	{"GET", "https://$HOST/wp-content/plugins/contact-form-7/images/ajax-loader.gif", auto_decompress=true},
    	{"POST", "https://$HOST/ga941638.js?PID=65B66C68-6EC9-3B18-99EC-5F2B8F8D3C20", headers={["X-Distil-Ajax"]="ebztqtrqtruyxfaadbqfy",["Content-Type"]="text/plain;charset=UTF-8"}, data="p=%7B%22appName%22%3A%22Netscape%22%2C%22platform%22%3A%22MacIntel%22%2C%22cookies%22%3A1%2C%22syslang%22%3A%22en-US%22%2C%22userlang%22%3A%22en-US%22%2C%22cpu%22%3A%22%22%2C%22productSub%22%3A%2220030107%22%2C%22setTimeout%22%3A0%2C%22setInterval%22%3A0%2C%22plugins%22%3A%7B%220%22%3A%22WidevineContentDecryptionModule%22%2C%221%22%3A%22ChromePDFViewer%22%2C%222%22%3A%22ShockwaveFlash%22%2C%223%22%3A%22ChromeRemoteDesktopViewer%22%2C%224%22%3A%22NativeClient%22%2C%225%22%3A%22ChromePDFViewer%22%7D%2C%22mimeTypes%22%3A%7B%220%22%3A%22WidevineContentDecryptionModuleapplication%2Fx-ppapi-widevine-cdm%22%2C%221%22%3A%22application%2Fpdf%22%2C%222%22%3A%22ShockwaveFlashapplication%2Fx-shockwave-flash%22%2C%223%22%3A%22FutureSplashPlayerapplication%2Ffuturesplash%22%2C%224%22%3A%22application%2Fvnd.chromium.remoting-viewer%22%2C%225%22%3A%22NativeClientExecutableapplication%2Fx-nacl%22%2C%226%22%3A%22PortableNativeClientExecutableapplication%2Fx-pnacl%22%2C%227%22%3A%22PortableDocumentFormatapplication%2Fx-google-chrome-pdf%22%7D%2C%22screen%22%3A%7B%22width%22%3A1440%2C%22height%22%3A900%2C%22colorDepth%22%3A24%7D%2C%22fonts%22%3A%7B%220%22%3A%22Calibri%22%2C%221%22%3A%22Cambria%22%2C%222%22%3A%22HoeflerText%22%2C%223%22%3A%22Monaco%22%2C%224%22%3A%22Constantia%22%2C%225%22%3A%22LucidaBright%22%2C%226%22%3A%22Georgia%22%2C%227%22%3A%22Candara%22%2C%228%22%3A%22TrebuchetMS%22%2C%229%22%3A%22Verdana%22%2C%2210%22%3A%22Consolas%22%2C%2211%22%3A%22AndaleMono%22%2C%2212%22%3A%22LucidaConsole%22%2C%2213%22%3A%22LucidaSansTypewriter%22%2C%2214%22%3A%22Monaco%22%2C%2215%22%3A%22CourierNew%22%2C%2216%22%3A%22Courier%22%7D%7D", auto_decompress=true}
    })

    http.request_batch({
    	{"GET", "https://maps.gstatic.com/maps-api-v3/api/js/21/3/common.js", headers={["X-Chrome-UMA-Enabled"]="1",["X-Client-Data"]="CJO2yQEIorbJAQiptskBCMS2yQEI6ojKAQidksoBCNGUygE="}, auto_decompress=true},
    	{"GET", "https://maps.gstatic.com/maps-api-v3/api/js/21/3/util.js", headers={["X-Chrome-UMA-Enabled"]="1",["X-Client-Data"]="CJO2yQEIorbJAQiptskBCMS2yQEI6ojKAQidksoBCNGUygE="}, auto_decompress=true},
    	{"GET", "https://maps.gstatic.com/maps-api-v3/api/js/21/3/stats.js", headers={["X-Chrome-UMA-Enabled"]="1",["X-Client-Data"]="CJO2yQEIorbJAQiptskBCMS2yQEI6ojKAQidksoBCNGUygE="}, auto_decompress=true},
    	{"GET", "https://maps.googleapis.com/maps/api/js/AuthenticationService.Authenticate?1shttps%3A%2F%2F$HOST%2Fbooking%2F%3Fswitch%3DRound%2520Trip%26from%3DONT%26to%3DBOS%26dateFrom%3D06%2F16%2F2015%252006%3A05%26dateTo%3D06%2F30%2F2015%252003%3A05%26adult%3D1%26child%3D2%26infant%3D0&5e1&callback=_xdc_._lg3jrk&token=73698", headers={["X-Chrome-UMA-Enabled"]="1",["X-Client-Data"]="CJO2yQEIorbJAQiptskBCMS2yQEI6ojKAQidksoBCNGUygE="}, auto_decompress=true},
    	{"GET", "https://csi.gstatic.com/csi?v=2&s=mapsapi3&action=apiboot2&rt=main.16", headers={["X-Chrome-UMA-Enabled"]="1",["X-Client-Data"]="CJO2yQEIorbJAQiptskBCMS2yQEI6ojKAQidksoBCNGUygE="}, auto_decompress=true}
    })

    http.request_batch({
    	{"POST", "https://$HOST/booking/?switch=Round+Trip&from=ONT&to=BOS&dateFrom=06%2F16%2F2015+06%3A05&dateTo=06%2F30%2F2015+03%3A05&adult=1&child=2&infant=0", headers={["X-Requested-With"]="XMLHttpRequest",["X-Distil-Ajax"]="ebztqtrqtruyxfaadbqfy",["Content-Type"]="application/x-www-form-urlencoded; charset=UTF-8"}, data="_wpcf7=663&_wpcf7_is_ajax_call=1&_wpcf7_locale=&_wpcf7_unit_tag=wpcf7-f663-p658-o2&_wpcf7_version=4.2&_wpnonce=2dbc9ac1d9&your_email=me%40gmail.com&your_fax=&your_name=My&your_phone=&your_requirements=Window%20Seat&your_surname=Name", auto_decompress=true},
    	{"GET", "https://$HOST/wp-content/themes/theme1770/images/icons/alert/icon-note.png", auto_decompress=true}
    })

    http.page_end("Page 1")

    client.sleep(math.random(20, 40))
```

* Each test case was run through: 
  + default User Agent: LoadImpactRload/3.0.8 (Load Impact; http://loadimpact.com);
  + Chrome User Agent
  + From the US
  + From Sao Palo, Brazil
  + From Singapore



##Commercial Applications

<span class="firstLetter">C</span>ommercial applications were used to fill the gap where the others left off. A commercial scraping application was used to simulate exactly what is available in the marketplace. Time was rented on a commercial botnet to simulate an actual legitimate botnet attack. Finally WPScan was used to do fine tuned brute force attacks on the victim site.

|Application          |Info                                    |Url                                                                              | 
|---------------------|----------------------------------------|---------------------------------------------------------------------------------|
| Automation Anywhere | Commercial screen scraping application | [{% image automation-anywhere-logo.jpeg %}](https://www.automationanywhere.com) |
| Commercial Botnet   | Time rented on a few commercial botnet of compromised hosts. |  |
| WPScan              | WordPress Scanning Application | [{% image wpscan-logo.png %}](https://wpscan.org)                                       |

###Commercial Application Configuration

<span class="firstLetter">W</span>PScan was executed with the following:
```
    ./wpscan.rb -r -v --url $HOST --wordlist /usr/share/SecLists/Passwords/darkc0de.txt --username admin
```
<span class="firstLetter">A</span>utomation Anywhere was configured with the following:
* Task #1
{% image automation_anywhere_config.png %}
* Task #2
{% image automation_anywhere_config2.png %}

