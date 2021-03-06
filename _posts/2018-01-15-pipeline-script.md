---
layout: page
title: "Pipeline script"
category: pipeline
date: 2018-01-05 15:17:55
order: 4
---
### Pipeline script summary

  In pipeline stage step definition, there is a step called script, which allows users to execute script in the server where IDA is deployed.

### Define script

  In **Edit Step** modal, select **Script** as **Type** then you can define one or more scripts in **Script** text area. For multiple scripts, each of them need to start from a new line.
  
  ![][pipeline_create_script]
  <br>
  <br>
  After the pipeline is executed, you can view the script execution result.
  
  ![][pipeline_script_result]  
  
### Script supported parameters
  
  IDA supports below parameters in Script. They can be used in Script to represent Pipeline related attributes.
  
  **${PIPELINE_NAME}**: current pipeline name
  <br>
  **${PIPELINE_ID}**: current pipeline ID
  <br>
  **${STAGE_NAME}**: current stage name
  <br>
  **${STEP_NAME}**: current step name
  <br>
  **${BUILD_ID}**: current build id
  <br>
  **${BUILD_REPORT_URL}**: current build report URL
  <br>
  **${APP_ACRONYM}**: current processApp acronym name
  <br>
  **${SNAPSHOT_ACRONYM}**: current snapshot acronym name
  
### Script samples
**Call RESTFul service**
  <br>
  <br>
   You can use **curl** to call a RESTful service or Web Service in Script. For example, below script calls a BPM REST API by curl.
  
  *curl -H "Accept:application/json" -H "Authorization:Basic YWRtaW46UGFzc3cwcmQ=" -k https://9.30.160.68:9444/rest/bpm/wle/v1/systems*
  <br>
  <br>
  **Call Web Service** 
  <br>
  <br>
  You can also use **curl** to call a Web Service.  For example, you can call a Web Service to send email notification during pipeline creation. Assume the Web Service is based on SOAP 1.2 and its WSDL URL is: http://enact1.fyre.ibm.com:9081/teamworks/webservices/HSS/SendEmailWS.tws?wsdl, you can use below curl script to call it.

*curl -H "Content-Type: application/soap+xml;charset=utf-8" -d "<soap:Envelope xmlns:soap='http://www.w3.org/2003/05/soap-envelope' xmlns:sen='https://enact1.fyre.ibm.com:9444/teamworks/webservices/HSS/SendEmailWS.tws'><soap:Header/><soap:Body><sen:send><sen:subject>${PIPELINE_NAME} result</sen:subject><sen:content>Please refer to ${BUILD_REPORT_URL}</sen:content><sen:to>ida@cn.ibm.com</sen:to><sen:cc>ida@cn.ibm.com</sen:cc></sen:send></soap:Body></soap:Envelope>" http://enact1.fyre.ibm.com:9081/teamworks/webservices/HSS/SendEmailWS.tws*
![][pipeline_email_script]

  <br> 
  **Call wsadmin command**
  <br>
  <br>
   You can execute a wsadmin command in Script. The wsadmin command is running against the BPM server associated to the Stage BPM configuration. For example,
  
  *ssh AdminTask.BPMSetEnvironmentVariable('[-containerAcronym ${APP_ACRONYM} -containerSnapshotAcronym ${SNAPSHOT_ACRONYM} -environmentVariableName TEST_KEY -environmentVariableValue 8899]')*
  
  This Script first logon BPM server using ssh, then execute the wsadmin commmand there to update the BPM environment variable. The format of the Script to call wsadmin command is 

**ssh** + space + **wsadmin command**

[pipeline_create_script]: ../images/pipeline/pipeline_create_script.png
[pipeline_script_result]: ../images/pipeline/pipeline_script_result.png 
[pipeline_email_script]: ../images/pipeline/pipeline_email_script.png 
