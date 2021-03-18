# Testing SOAP

There are several ways to test SOAP requests, depending on the available tools, expertise and preference of the user. SoapUI for example could be used if a tool with a graphical user interface is prefered. Another option is to use cURL. cURL is a command line tool and is available for almost every operating system. Curl can be downloaded from [http://curl.haxx.se/dlwiz/?type=bin](http://curl.haxx.se/dlwiz/?type=bin).

In the example below the file soaprequest.xml contains the soap call to export data and the response will be written to the file soapresponse.xml

```text
curl --data-binary @soaprequest.xml -o soapresponse.xml -H "SOAPACTION: http://tempuri.org/SoapConnector/ExportData" -H "Content-type: text/xml;charset=utf-8" 
       https://customer.ultimo.net/Webservices/SoapConnector.svc
```

To import data the SOAPACTION should be changed to ImportData.

```text
curl --data-binary @soaprequest.xml -o soapresponse.xml -H "SOAPACTION: http://tempuri.org/SoapConnector/ImportData" -H "Content-type: text/xml;charset=utf-8" 
       https://customer.ultimo.net/Webservices/SoapConnector.svc

```

### 

