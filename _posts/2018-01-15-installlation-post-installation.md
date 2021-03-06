---
layout: page
title: "Post Installation Setup"
category: installation
date: 2018-01-05 15:17:55
order: 6
---



### Selenium Grid

IDA integrates with remote testing automation framework based on Selenium Grid.

##### Notes

Below is the reference link for how to setup selenium grid.It includes the detail parameter setting explanation.   
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

	- Create hubconfig.json
		```
		{
		  "port": 4444,
		  "newSessionWaitTimeout": 10000,
		  "servlets" : [],
		  "withoutServlets": [],
		  "custom": {},
		  "capabilityMatcher": "org.openqa.grid.internal.utils.DefaultCapabilityMatcher",
		  "registry": "org.openqa.grid.internal.DefaultGridRegistry",
		  "throwOnCapabilityNotPresent": true,
		  "cleanUpCycle": 5000,
		  "role": "hub",
		  "debug": false,
		  "browserTimeout": 60,
		  "timeout": 60
		}
		```
	- Run the command
		```
		java -jar selenium-server-standalone-3.14.0.jar -role hub -hubConfig hubconfig.json
		```

4. Start the Node  

	- Create nodeconfig.json
		```
		{
		  "capabilities":
		  [
		    {
		      "browserName": "firefox",
		      "marionette": true,
		      "maxInstances": 5,
		      "seleniumProtocol": "WebDriver",
			  "version": 57
		    },
		    {
		      "browserName": "chrome",
		      "maxInstances": 5,
		      "seleniumProtocol": "WebDriver",
			  "version": 68
		    },
		    {
		      "browserName": "internet explorer",
		      "platform": "WINDOWS",
		      "maxInstances": 1,
		      "seleniumProtocol": "WebDriver",
			  "version": 11
		    }
		  ],
		  "proxy": "org.openqa.grid.selenium.proxy.DefaultRemoteProxy",
		  "maxSession": 5,
		  "port": -1,
		  "register": true,
		  "registerCycle": 5000,
		  "hub": "http://localhost:4444",
		  "nodeStatusCheckTimeout": 5000,
		  "nodePolling": 5000,
		  "role": "node",
		  "unregisterIfStillDownAfter": 60000,
		  "downPollingLimit": 2,
		  "debug": false,
		  "servlets" : [],
		  "withoutServlets": [],
		  "custom": {},
		  "browserTimeout": 60,
		  "timeout": 60
		}
		```
	- Run the command
		```
		java -jar selenium-server-standalone-3.14.0.jar -role node -nodeConfig nodeconfig.json
		```

**Docker installation**

The followingThe below link is shows a step-by-step introduction to using the official Selenium Docker images using a basic selemium hub and selenium/ node configuration.

[How to get started selenium hub and node](https://github.com/SeleniumHQ/docker-selenium/wiki/Getting-Started-with-Hub-and-Nodes)

You can follow below script to install Selenium Hub and Node on docker.We recommand you to install debug image version so you can visually see what the browser is doing.

``` 
docker pull selenium/hub:3.14.0-gallium    
docker pull selenium/node-firefox-debug:3.14.0-gallium 
docker pull selenium/node-chrome-debug:3.14.0-gallium    


docker network create grid
docker run -d -p 4444:4444 --net grid --name selenium-hub selenium/hub:3.14.0-gallium    
docker run -d --net grid -e HUB_HOST=selenium-hub  -v /dev/shm:/dev/shm selenium/node-chrome-debug:3.14.0-gallium 
docker run -d --net grid -e HUB_HOST=selenium-hub  -v /dev/shm:/dev/shm selenium/node-firefox-debug:3.14.0-gallium 

``` 
You can do the below command mutiple times,it will create mutiple chrome/firefox node for the grid remote testing. It can provide you to run the  selenium tesing parallel in the Grid.We recommand you to create above  5 node number for chrome/firefox each.   

``` 
docker run -d --net grid -e HUB_HOST=selenium-hub  -v /dev/shm:/dev/shm selenium/node-chrome-debug:3.14.0-gallium 
docker run -d --net grid -e HUB_HOST=selenium-hub  -v /dev/shm:/dev/shm selenium/node-firefox-debug:3.14.0-gallium 

``` 
After that you can open the browser http://dockerhostIP:4444/grid/console to check selenium grid/node  installation status.    

  ![][selenium_node]

### Install IDA Toolkit
The testing capability can only start exposed Business Process, Human Services and AJAX Services.  If you wish to directly test other services such as system services, integration services or business processes which are not exposed then you need to install the IDA Toolkit.

1. Import the IDA_Toolkit - 8.6.0_v1.1.twx from IDA toolkit folder into the proces center.
2. Add the IDA toolkit as a this toolkit dependency for within your process application.
3. Right click the "IDA Utility" service flow and copy the item to your process app.  

   ![][toolkit]
4. Make sure the service is installed in your process app.  
   ![][service]
   
### Install IDA Plug-in

#### Chrome
- Open the url <a href="https://chrome.google.com/webstore/search/IDA%20IBM" target="_blank">https://chrome.google.com/webstore/search/IDA%20IBM</a>
- Click "Add to Chrome" button to install plug-in

#### Firefox
- Download firefox plugin [ida-1.25-fx.xpi](../plugins/ida-1.25-fx.xpi)
- Drag the "ida-1.25-fx.xpi" file into firefox window
- Click "Add" button

**Plug-in Configuration**

If you want to use the checkstyle and codereivew feature on web PD, then you need to set the IDA url and user credentials for the plug-in options. 
the IDA URL: https://9.30.255.220:9443/IDA   
the username: the IDA login username 
the password: the IDA login password.   

   ![][IDAOption]


[toolkit]: ../images/install/toolkit.png 
[service]: ../images/install/service.png 
[IDA]: ../images/install/IDA.png 
[firefox]: ../images/install/firefox.png
[seleniumGrid]: ../images/install/seleniumGrid.png
[webDriver]: ../images/install/webdriver.png
[IDAOption]: ../images/install/IDAOption.png

### Self-Signed SSL Certificates Installation

The IDA recorder plugin can't support website with self sign certification by default. In this case, a warning like this:
   
![][error]
   
   This warning will block the recording of test case. To resolve this problem, we need to make the browsers to accept self-signed certificate.    
   
#### FireFox - Add a Security Exception

1. In FireFox, go to Tools -> Options.

    ![][tool]

2. Click the **Privaty & Security** tab,  then the **View Certificates** button.

    ![][security_tab]
    
3. Go to the **Services** tab and press the **Add Exception** button.
    
    ![][servers_tab]
    
4.  Enter the host and port in **Add Security Exception** dialog, press  **Get Certificate** button, check the box near the bottom **Permanently store this exception** and press **Confirm Security Exception** .

     ![][add_security]
     
    From this point on, FireFox won't show SSL-related errors. when visiting the website, it will like this:
    
    ![][success]
    
#### Chrome - Visit in unsafe mode

Chrome browsers can save your data for a short time, and the warning page will not appear and block recording if you visit the testing website in unsafe mode before recording.

1. Click **ADVANCE** in warning page.

    ![][chrome_error]
    
2. Click **Proceed to 9.30.160.68(unsafe)**.

    ![][proceed]
     



   
   [error]: ../images/install/installation_self_signed_sertificates_error.png 
   [tool]: ../images/install/installation_self_signed_sertificates_tool.png 
   [security_tab]: ../images/install/installation_self_signed_sertificates_security_tab.png
   [servers_tab]: ../images/install/installation_self_signed_sertificates_servers_tab.png
   [add_security]: ../images/install/installation_self_signed_sertificates_add_security.png
   [success]: ../images/install/installation_self_signed_sertificates_success.png 
   [chrome_error]: ../images/install/installation_self_signed_sertificates_chrome_error.png
   [proceed]: ../images/install/installation_self_signed_sertificates_proceed.png
   [selenium_node]: ../images/install/selenium_node.png

### Monitoring configuration

You could refer to [Monitoring configuration](../administration/administration_monitoring configuration.html) to enable monitoring feature of IDA.
