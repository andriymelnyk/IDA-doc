---
layout: page
title: "Post Installation Setup"
category: installation
date: 2018-01-05 15:17:55
order: 5
---



### Selenium Grid

Keter integrates with remote testing automation framework based on Selenium Grid.

##### Notes
- [Selenium Grid Setup Guidance](https://github.com/SeleniumHQ/selenium/wiki/Grid2)  

**Local installation**

1. Download [Selenium 3.14.0](https://selenium-release.storage.googleapis.com/3.14/selenium-server-standalone-3.14.0.jar)  

2. Download Selenium web drivers and put them under the same folder of the Selenium Jar file.
- [IE Driver](http://selenium-release.storage.googleapis.com/3.14/IEDriverServer_Win32_3.14.0.zip)
- [Firefox Driver Win64](https://github.com/mozilla/geckodriver/releases/download/v0.23.0/geckodriver-v0.23.0-win64.zip)
- [Firefox Driver Linux64](https://github.com/mozilla/geckodriver/releases/download/v0.23.0/geckodriver-v0.23.0-linux64.tar.gz)
- [Chrome Driver Win32](https://chromedriver.storage.googleapis.com/2.41/chromedriver_win32.zip)  
- [Chrome Driver Linux64](https://chromedriver.storage.googleapis.com/2.41/chromedriver_linux64.zip)    

3. Start the Hub
	```
	java -jar selenium-server-standalone-3.14.0.jar -role hub -newSessionWaitTimeout 10000 -timeout 60 -browserTimeout 60
	```

4. Start the Node  
	```
	java -jar selenium-server-standalone-3.14.0.jar -role node -hub http://localhost:4444/grid/register/ -timeout 60 -browserTimeout 60
	```

**Docker installation**

The followingThe below link is shows a step-by-step introduction to using the official Selenium Docker images using a basic selemium hub and selenium/ node configuration.

[How to get started selenium hub and node](https://github.com/SeleniumHQ/docker-selenium/wiki/Getting-Started-with-Hub-and-Nodes)

### Install Keter Toolkit
The testing capability can only start exposed Business Process, Human Services and AJAX Services.  If you wish to directly test other services such as system services, integration services or business processes which are not exposed then you need to install the Keter Toolkit.

1. Import the Keter_Toolkit - 8.6.0_v1.0.twx from Keter toolkit folder into the proces center.
2. Add the keter toolkit as a this toolkit dependency for within your process application.
3. Right click the "Keter Utility" service flow and copy the item to your process app.  

   ![][toolkit]
4. Make sure the service is installed in your process app.  
   ![][service]
   
### Plug-in

#### Chrome
- Open the url <a href="https://chrome.google.com/webstore/search/keter" target="_blank">https://chrome.google.com/webstore/search/keter</a>
- Click "Add to Chrome" button to install plug-in

#### Firefox
- Download firefox plugin [keter-1.22-fx.xpi](../plugins/keter-1.22-fx.xpi)
- Drag the "keter-1.22-fx.xpi" file into firefox window
- Click "Add" button

**Plug-in Configuration**

If you want to use the checkstyle and codereivew feature on web PD, then you need to set the Keter url and user credentials for the plug-in options. 
the Keter URL: https://9.30.255.220:9443/keter   
the username: the Keter login username 
the password: the Keter login password.   

   ![][keterOption]


[toolkit]: ../images/install/toolkit.png 
[service]: ../images/install/service.png 
[keter]: ../images/install/keter.png 
[firefox]: ../images/install/firefox.png
[seleniumGrid]: ../images/install/seleniumGrid.png
[webDriver]: ../images/install/webdriver.png
[keterOption]: ../images/install/keterOption.png



