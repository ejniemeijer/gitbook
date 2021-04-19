# SOAP

🚫 Professional ✅ Premium ✅ Enterprise

This section explains how to setup a connection to the WCF / SOAP service.

### How to connect

The url to send the request to:

`https://customer.ultimo.net/WebServices/SoapConnector.svc`

* `https://customer.ultimo.net/` 

  is the domain name where the Ultimo software is available.  

* `/WebServices/SoapConnector.svc` 

  is a relative path, if the Ultimo software is installed in a different directory.

_Example:_

Ultimo Software is located at:  
`https://customer.ultimo.net/`

Then the path should be:  
`https://customer.ultimo.net/Webservices/SoapConnector.svc`

### WSDL

Available functionality within the SOAP web service can be found inside the WSDL description.

`https://customer.ultimo.net/WebServices/SoapConnector.svc?wsdl`

### Authentication

When sending a request to the SOAP web service, authentication should be added to the SOAP body.



