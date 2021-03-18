# Testing HTTP POST

There are several ways to test HTTP Post requests, depending on the available tools, expertise and preference of the user. Postman for example could be used if a tool with a graphical user interface is prefered. Another option is to use cURL. cURL is a command line tool and is available for almost every operating system. Curl can be downloaded from [http://curl.haxx.se/dlwiz/?type=bin](http://curl.haxx.se/dlwiz/?type=bin).

In the example below the file test.xml contains the data and the response will be written to the file response.xml

```http
curl --data-binary @c:\test.xml -o c:\response.xml -H "Content-type: text/xml;charset=utf-8" 
     "https://customer.ultimo.net/Webservices/Connector.ashx?username=<<username>>&password=<<password>>&action=export&exportName=<<exportconnectorname>>"
```

To import data the action should be changed to import:

```text
curl --data-binary @c:\test.xml -o c:\response.xml -H "Content-type: text/xml;charset=utf-8" 
     "https://customer.ultimo.net/Webservices/Connector.ashx?username=<<username>>&password=<<password>>&action=import&importName=<<importconnectorname>>"
```

### 

