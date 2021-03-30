# HTTP POST

It’s possible to communicate to the HTTP handler through an HTTP POST request. Within the HTTP POST specific data using key-value pairs are required in the message and send an XML string as a single parameter that includes all the objects structured in XML format.

The receiving client should parse the HTTP POST response data and extract the XML that was constructed by Ultimo Business Integration containing all the object data.

### How to connect

The url to send the request to:

`http://customer.ultimo.net/WebServices/Connector.ashx`

* `http://customer.ultimo.net` 

  represents the domain name where the Ultimo software is available.

* `/WebServices/Connector.ashx`  represents a relative path, if the Ultimo software is installed in a different directory.

_Example:_

Ultimo Software is located at:  
`http://customer.ultimo.net/Ultimo/`

Then the path should be:  
`http://customer.ultimo.net/Ultimo/Webservices/Connector.ashx`

### Authentication

When sending a request using HTTP POST, authentication should be added as username & password parameters to the connection string. Therefor it is strongly recommended to use a HTTPS connection when using HTTP Post.

