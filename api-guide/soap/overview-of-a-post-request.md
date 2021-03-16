# Overview of a POST request

The following example explains the raw POST request as it is sent to SoapConnector.svc:

```markup
POST /WebServices/SoapConnector.svc HTTP/1.1
Host: <>
Content-Type: text/xml;charset=UTF-8
SOAPAction: "http://tempuri.org/SoapConnector/ExportData"
Content-Length: <<length_of_data>>
Connection: close

```

Several nodes should be added to the SOAP body to make it more specific. Which parameters to use depend on the desired action.

| Parameter | Value |
| :--- | :--- |
| userName | An existing user in Ultimo \(Id\) with authorisation to execute connectors |
| password | Password of the above user |
| exportName | Required when action is 'export'. Id of the export connector. |
| importName | Required when action is 'import'. Id of the import connector. |

