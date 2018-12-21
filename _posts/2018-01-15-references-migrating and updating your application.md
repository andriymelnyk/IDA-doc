---
layout: page
title: "Migrating and updating IDA Application"
category: references
date: 2018-11-06 15:17:55
order: 5
---

### Preparing your migration

To prepare your migration, take the following steps:  

1. Download the lastested IDA version.
2. Stop the libery server.  
3. Stop the mysql server.  
4. Backup the mysql db.    

### Step 1: Update DB

To update DB, take the following steps: 

1. Start the mysql Server.  
2. Find migrate-mysql.sql in the sql folder. Copy the corresponding version sql into clipboard.   

![][mysqlmigration]   

3. Connect to the MySQL server and use IDA database.You can execute the script from the  clipboard. 

``` 
mysql> use IDA ;   
mysql> paste your sql here   
```    

### Step 2: Update ida-web.war   

For IDA version migration, you need to update ida-web.war, take the following steps: 

1. Find  ida.properties under conf folder in the previous version.
2. Reconfigure  ida.properties file in the new version(in the new folder structure you downloaded). Make sure every property value in the new ida.properties file equals to the corresponding value in the old ida.properties file. After you finished the properties value change, compare two files and make sure property values are the same.  
3. Copy MySQL-connector-java-5.1.44.jar file to the Lib folder of the new IDA version folder structure. The jar itself can be copied either from the old IDA Lib folder or from the official my sql web site. 
4. Execute package.bat command to repackage the ida-web.war file with new settings you just made(you do this in the new IDA folder structure). The updated ida-web.war file created under the build folder.
5. Remove all the files from wlp(Liberty) installation location\usr\servers\default\apps folder.     
6. Copy the ida-web.war (the one generated in step4) into the wlp(Liberty) installation\usr\servers\default\apps folder.    
7. Start the Liberty Server.  

 **Notes**     
 Please not overwrite ida.properties from previous version, since we might add some new property name  in new IDA versions.  
 You can check the application-prod.yml in ida-web.war to make sure these setting are applied.


### Step 3: Update Keter 1.3.x to IDA 2.0.0(optional)

If you want to migrate keter to to IDA version, take the following steps:     

1. Open the server.xml of liberty and make sure keter-web.war is changed to ida-web.war and application id and name is also changed 
to ida. If your liberty has setup keyStore password before,you don;t need to change defaultKeyStore password, just keep orignal one,otherwise you need to create a passowrd for the defaultKeyStore.            
   

```                     
    <!-- Automatically expand WAR files and EAR files -->
    <applicationManager autoExpand="true" startTimeout="360s" stopTimeout="120s"/> 
	<application type="war" id="ida" name="ida" location="${server.config.dir}/apps/ida-web.war">
		<classloader delegation="parentLast" />
    </application>
	
	<keyStore id="defaultKeyStore" password="idaAdmin" />

``` 
2. Use IDA toolkit to replace Keter toolkit for the BPM.Both toolkit are compatible for usage.      
3. Use latested chrome and fixfox plug-in.       

 **Notes**     
 Please not overwrite ida.properties from previous version since we might add new some new property name  in some versions.   

[yamlmigration]: ../images/install/productionyaml.png
[mysqlmigration]: ../images/install/mysqlmigration.png
