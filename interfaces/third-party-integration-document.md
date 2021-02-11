# Third party integration document

![](../.gitbook/assets/image.webp)

Ultimo Business Integration can be described as:

* A default integration platform
* Offering import & export functionalities
* Based on XML, CSV or JSON
* Accessible via web services

Ultimo Business Integration offers:

* Various default integrations
* Support for standards like LDAP and OCI
* Additional Interfaces created by our consultants
* Integration with files or web service requests
* Possibilities for XSL transformation
* Default logging and rollback mechanisms

## What is Ultimo Business Integration 

Ultimo Business Integration provides an API to Ultimo for third party applications. The API offers the following functions:

* Insert data
* Update data
* Delete data
* Retrieve data

Ultimo is able to process the received data using the built in business logic. This can be configured by our consultants.

## How to access

The following types of communications are supported to access the API:

* HTTP POST
* SOAP
* REST

More info about each type of communication are described in the following paragraphs.

### HTTP POST 

It’s possible to communicate to the HTTP handler through an HTTP POST request. Within the HTTP POST specific data using key-value pairs are required in the message and send an XML string as a single parameter that includes all the objects structured in XML format.

The receiving client should parse the HTTP POST response data and extract the XML that was constructed by Ultimo Business Integration containing all the object data. The object data the is returned can be configured by our consultants.

#### How to connect with HTTP POST 

The url to send the request to: [_http://internaldomain.org/WebServices/Connector.ashx_](http://internaldomain.org/WebServices/Connector.ashx)

The \*[http://internaldomain.org\*](http://internaldomain.org*/) represents the domain name where the Ultimo software is available.

_/WebServices/Connector.ashx_ represents a relative path, if the Ultimo software is installed in a different directory.

_Example:_

Ultimo Software is located at: [http://internaldomain.org/Ultimo/](http://internaldomain.org/Ultimo/)

Then the path should be: [http://internaldomain.org/Ultimo/Webservices/Connector.ashx](http://internaldomain.org/Ultimo/Webservices/Connector.ashx)

#### Authentication

When sending a request using HTTP POST, authentication should be added as username & password parameters to the connection string. Therefor it is strongly recommended to use a HTTPS connection when using HTTP Post.

#### Overview of a POST request 

The following example explains the raw POST request as it is sent to Connector.ashx:

```text
POST  /WebServices/Connector.ashx HTTP/1.1
Host: <>
Content-type: application/x-www-form-urlencoded
Content-length: <<length_of_data>>
Connection: close

Several parameter should be added to the request to make it more specific. Which parameters to use depend on the desired action.
```

<table>
  <thead>
    <tr>
      <th style="text-align:left">Parameter</th>
      <th style="text-align:left">Value</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">Username</td>
      <td style="text-align:left">An existing user in Ultimo (Id) with authorisation to execute connectors</td>
    </tr>
    <tr>
      <td style="text-align:left">Password</td>
      <td style="text-align:left">Password of the above user</td>
    </tr>
    <tr>
      <td style="text-align:left">Action</td>
      <td style="text-align:left">
        <p>&apos;export&apos; when retrieving data from Ultimo</p>
        <p>&apos;import&apos; when changing data in Ultimo</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">ExportName</td>
      <td style="text-align:left">Required when action is &apos;export&apos;. Id of the export connector.</td>
    </tr>
    <tr>
      <td style="text-align:left">ImportName</td>
      <td style="text-align:left">Required when action is &apos;import&apos;. Id of the import connector.</td>
    </tr>
  </tbody>
</table>

#### Requesting data 

It is possible to retrieve data by sending a request to one of the activated export connectors. Without sending a body, all data from the connector will be returned.

_Parameter example:_

username=user&password=password&action=export&exportName=exportconnector

It is possible to add filters to a request to retrieve specific information from the connector. Filters can be added by sending them in the body of the request.

_Example body_

```text
<?xml version="1.0" encoding="utf-8"?>
<Data>
	<Object Type="EntityName" Action="InsertOrUpdate">
		<Property Name="Id" Value="CODE01" />
		<Property Name="Description" Value="Object 1" />
	</Object>       

	<Object Type="EntityName" Action="InsertOrSkip">
		<Property Name="Id" Value="CODE02" />
		<Property Name="Description" Value="Object 2" />

		<Object Type="EntityName" Action="InsertOrSkip">
			<Property Name="Id.Parent.Id" Value="${Parent.Id}" Evaluate="true" />
			<Property Name="Id.Child" Value="CODE03" />
			<Property Name="Description" Value="Nested object" />
		</Object>
	</Object>             
</Data>
```

_Example response_

Returns the full request or an exception

```text

<?xml version="1.0" encoding="utf-16"?>
<Data>
	<Object Type="EntityName" Action="Insert">
		<Property Name="Id" Value="CODE01" />
	</Object>
	<Object Type="EntityName" Action="Insert">
		<Property Name="Id" Value="CODE02" />
	</Object>
	<Object Type="EntityName" Action="Insert">
		<Property Name="Id" Value="CODE03" />
	</Object>
</Data>

```

The request and response are expected and returned in a fixed Ultimo format. With XSL transformations, it is possible to transform the request \(to the Ultimo format\) and response \(to the desired custom format\) when using XML as an output result. This is optional and can only be done by our consultants. Output can also be returned in a default CSV or JSON format.

#### Inserting data 

It is possible to change data by sending a request to one of the activated import connectors.

_Parameter example:_

username=user&password=password&action=import&importName=importconnector

Each object to import should be bundled in a single &lt;object&gt; node. All attributes of an object should be added as properties. The name of a property represents a database field in Ultimo. It is possible to send multiple objects at once. The action to execute per object should be determined when creating an interface.

It is also possible to nest objects, to create a parent/child construction. When nested objects are used, the ${Parent.Id} syntax can be used to refer to the parent. To make sure Ultimo tries to get the value of the parent, attribute Evaluation should be added.

_Example body including a nested object_

_Example response_

Returns the full request or an exception

```text
<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/" xmlns:tem="http://tempuri.org/">
   <soapenv:Header/>
   <soapenv:Body>
      <tem:ImportData>
         <tem:importName>importconnector</tem:importName>
         <tem:userName>user</tem:userName>
         <tem:password>password</tem:password>
         <tem:importData><![CDATA[<Data>
        <Object Type="EntityName" Action="InsertOrUpdate">
                <Property Name="Id" Value="CODE01" />
                <Property Name="Description" Value="Object 1" />
        </Object>       
 
        <Object Type="EntityName" Action="InsertOrSkip">
                <Property Name="Id" Value="CODE02" />
                <Property Name="Description" Value="Object 2" />
 
                <Object Type="EntityName" Action="InsertOrSkip">
                        <Property Name="Id.Parent.Id" Value="${Parent.Id}" Evaluate="true" />
                        <Property Name="Id.Child" Value="CODE03" />
                        <Property Name="Description" Value="Nested object" />
                </Object>
        </Object>             
</Data>]]></tem:importData>
      </tem:ImportData>
   </soapenv:Body>
</soapenv:Envelope>
```

```text
<s:Envelope xmlns:s="http://schemas.xmlsoap.org/soap/envelope/">
	<s:Body>
		<ImportDataResponse xmlns="http://tempuri.org/">
			<ImportDataResult>
				<![CDATA[<?xml version="1.0" encoding="utf-16"?>
				<Data>
					<Object Type="Equipment" Action="Insert">
						<Property Name="Id" Value="CODE01" />
					</Object>
					<Object Type="Equipment" Action="Insert">
						<Property Name="Id" Value="CODE02" />
					</Object>
					<Object Type="Equipment" Action="Insert">
						<Property Name="Id" Value="CODE03" />
					</Object>
				</Data>]]>
			</ImportDataResult>
		</ImportDataResponse>
	</s:Body>
</s:Envelope>
```

#### Testing HTTP Post

There are several ways to test HTTP Post requests, depending on the available tools, expertise and preference of the user. Postman for example could be used if a tool with a graphical user interface is prefered. Another option is to use cURL. cURL is a command line tool and is available for almost every operating system. Curl can be downloaded from [http://curl.haxx.se/dlwiz/?type=bin](http://curl.haxx.se/dlwiz/?type=bin).

In the example below the file test.xml contains the data and the response will be written to the file response.xml

```text
curl --data-binary @c:\test.xml -o c:\response.xml -H "Content-type: text/xml;charset=utf-8" 
     "http://internaldomain.org/Ultimo/Webservices/Connector.ashx?username=<<username>>&password=<<password>>&action=export&exportName=<<exportconnectorname>>"
```

To import data the action should be changed to import:

```text
curl --data-binary @c:\test.xml -o c:\response.xml -H "Content-type: text/xml;charset=utf-8" 
     "http://internaldomain.org/Ultimo/Webservices/Connector.ashx?username=<<username>>&password=<<password>>&action=import&importName=<<importconnectorname>>"
```

### SOAP 

This section explains how to setup a connection to the WCF / SOAP service.

#### How to connect with SOAP 

The url to send the request to: [_http://internaldomain.org/WebServices/SoapConnector.svc_](http://internaldomain.org/WebServices/SoapConnector.svc)

[_http://internaldomain.org_](http://internaldomain.org/) is the domain name where the Ultimo software is available.

_/WebServices/SoapConnector.svc_ is a relative path, if the Ultimo software is installed in a different directory.

_Example:_

Ultimo Software is located at: [_http://internaldomain.org/Ultimo/_](http://internaldomain.org/Ultimo/)

Then the path should be: [_http://internaldomain.org/Ultimo/Webservices/SoapConnector.svc_](http://internaldomain.org/Ultimo/Webservices/SoapConnector.svc)

#### WSDL description 

Available functionality within the SOAP web service can be found inside the WSDL description.

[_http://internaldomain.org/WebServices/SoapConnector.svc?wsdl_](http://internaldomain.org/WebServices/SoapConnector.svc?wsdl)

#### Authentication

When sending a request to the SOAP web service, authentication should be added to the SOAP body.

#### Overview of a POST request 

The following example explains the raw POST request as it is sent to SoapConnector.svc:

```text
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

#### Requesting data

It is possible to retrieve data by sending a request to one of the activated export connectors. Without sending requestData, all data from the connector will be returned. It is possible to add filters to a request to retrieve specific information from the connector. Filters can be applied by adding them in the requestData node within a CDATA block.

_Example request_

|  |
| :--- |


_Example response_

Returns the full request or an exception

|  |
| :--- |


The request and response are expected and returned in a fixed Ultimo format. With XSL transformations, it is possible to transform the request \(to the Ultimo format\) and response \(to the desired custom format\) when using XML as an output result. This is optional and can only be done by our consultants. Output can also be returned in a default CSV or JSON format.

#### Inserting data

It is possible to change data by sending a request to one of the activated import connectors.

Each object to import should be bundled in a single &lt;object&gt; node. All attributes of an object should be added as properties. The name of a property represents a database field in Ultimo. It is possible to send multiple objects at once. The action to execute per object should be determined when creating an interface.

It is also possible to nest objects, to create a parent/child construction. When nested objects are used, the ${Parent.Id} syntax can be used to refer to the parent. To make sure Ultimo tries to get the value of the parent, attribute Evaluation should be added.

_Example request including a nested object_

|  |
| :--- |


_Example response_

Returns the full request or an exception

|  |
| :--- |


#### Testing SOAP

There are several ways to test SOAP requests, depending on the available tools, expertise and preference of the user. SoapUI for example could be used if a tool with a graphical user interface is prefered. Another option is to use cURL. cURL is a command line tool and is available for almost every operating system. Curl can be downloaded from [http://curl.haxx.se/dlwiz/?type=bin](http://curl.haxx.se/dlwiz/?type=bin).

In the example below the file soaprequest.xml contains the soap call to export data and the response will be written to the file soapresponse.xml

```text
curl --data-binary @soaprequest.xml -o soapresponse.xml -H "SOAPACTION: http://tempuri.org/SoapConnector/ExportData" -H "Content-type: text/xml;charset=utf-8" 
       http://internaldomain.org/Ultimo/Webservices/SoapConnector.svc
```

To import data the SOAPACTION should be changed to ImportData.

```text
curl --data-binary @soaprequest.xml -o soapresponse.xml -H "SOAPACTION: http://tempuri.org/SoapConnector/ImportData" -H "Content-type: text/xml;charset=utf-8" 
       http://internaldomain.org/Ultimo/Webservices/SoapConnector.svc

```

### REST

As of Ultimo version 18.04, the possibility to connect to a REST API has been added to Ultimo.

#### Authentication

When sending a request to the REST API, an API key should be provided in the header of the request. The API key can be generated in Ultimo.

#### Requesting data

With the GET request method it is possible for API clients to retrieve information from Ultimo.

The basic URL for requesting entities has the following format:

![](https://ultimo.atlassian.net/wiki/download/attachments/19005527/accolades_rest_api.png?version=1&modificationDate=1590562897188&cacheVersion=1&api=v2)

The service root is the same for every request or query. It contains the environment name, which is a string to identify your Ultimo environment. The resource path refers to the specific information you request. The query options determine which parts of this information are presented and how.

**Headers**

The headers of a request contains general configurations. It contains the following options:

* The API key must be specified in the following format: ApiKey: {API KEY}
* You can specify in which language you receive the response. If not specified the default user language is returned. The language is returned in the header of the response. You can specify the language in the following format: Accept-language: NL

**Resource paths**

The next examples explain the different uses of resource paths:

* Retrieve a list of entities by using an entity set: GET [http://customer.ultimo.com/api/v1/object/Equipment/](http://customer.ultimo.com/api/v1/object/Equipment/)
* Retrieve a list of custom entities: GET [http://customer.ultimo.com/api/V1/Object/\_Entity/](http://customer.ultimo.com/api/V1/Object/_Entity/)
* Retrieve a single entity by using an entity key: GET [http://customer.ultimo.com/api/v1/object/Equipment\('00001'\)](http://customer.ultimo.com/api/v1/object/Equipment%28)
* Retrieve a single entity with a composite key by using multiple key properties: GET [http://customer.ultimo.com/api/v1/object](http://customer.ultimo.com/api/v1/object) /EquipmentSparePart\(Equipment='00001',Article='0191'\)
* The resource path can be nested so it is possible to retrieve a single entity by following a navigation property from a single entity to another single entity: GET [http://customer.ultimo.com/api/v1/object](http://customer.ultimo.com/api/v1/object) /Equipment\('00001'\)/Department
* Or by following a navigation property from a single entity to a collection of entities: GET [http://customer.ultimo.com/api/v1/object](http://customer.ultimo.com/api/v1/object) /Department\('01'\)/Jobs
* The path segments can be composed recursively: GET [http://customer.ultimo.com/api/v1/object](http://customer.ultimo.com/api/v1/object) /Department\('01'\)/Jobs\('0000002'\)/Equipments
* When retrieving an entity with multiple key properties that includes a navigation property of the related entity the key properties can be omitted: GET [http://customer.ultimo.com/api/v1/object](http://customer.ultimo.com/api/v1/object) /Building\('0001'\)/BuildingParts\(Building='0001',Id='001'\) /BuildingFloor
* The previous example can be shortened to: GET [http://customer.ultimo.com/api/v1/object](http://customer.ultimo.com/api/v1/object) /Building\('0001'\)/BuildingParts\('001'\)/BuildingFloor

**Query options**

The query options are based on the OData version 4 standard query options. The following options are supported:

Filter

The 'filter' query option can be used to filter the collection of entities. The following operators can be used to filter:

| Note |
| :--- |
| The spaces around the operators used in the following examples must be percent-encoded as %20 when used in a real URL. |

* Equals: GET [http://customer.ultimo.com/api/v1/object](http://customer.ultimo.com/api/v1/object) /Article?filter=Description eq 'Luchtfilter' The 'eq' operator returns true if the left operand is equal to the right operand, otherwise it returns false.
* Not Equals: GET [http://customer.ultimo.com/api/1/object](http://customer.ultimo.com/api/1/object) /Article?filter=Description ne 'Luchtfilter' The 'ne' operator returns true if the left operand is not equal to the right operand, otherwise it returns false.
* Greater Than: GET [http://customer.ultimo.com/api/V1/object](http://customer.ultimo.com/api/V1/object) /Article?filter=PurchasePrice gt 500.0 The 'gt' operator returns true if the left operand is greater than the right operand, otherwise it returns false.
* Greater Than or Equal: GET [http://customer.ultimo.com/api/v1/object](http://customer.ultimo.com/api/v1/object) /Article?filter=PurchasePrice ge 500.0 The 'ge' operator returns true if the left operand is greater than or equal to the right operand, otherwise it returns false.
* Less Than: GET [http://customer.ultimo.com/api/v1/object](http://customer.ultimo.com/api/v1/object) /Article?filter=PurchasePrice lt 1.0 The 'lt' operator returns true if the left operand is less than the right operand, otherwise it returns false.
* Less Than or Equal: GET [http://customer.ultimo.com/api/v1/object](http://customer.ultimo.com/api/v1/object) /Article?filter=PurchasePrice le 1.0 The 'le' operator returns true if the left operand is less than or equal to the right operand, otherwise it returns false.
* Like: GET [http://customer.ultimo.com/api/v1/object](http://customer.ultimo.com/api/v1/object) /Article?filter=Description like 'Lucht%' The 'like' operator returns true if the left operand matches the pattern of the right operand, otherwise it returns false. The % wildcard matches any string of zero or more characters. The \_ wildcard matches any single character.
* And GET [http://customer.ultimo.com/api/v1/object](http://customer.ultimo.com/api/v1/object) /Article?filter=PurchasePrice gt 1000.0 and PurchasePrice lt 1500.0 The 'and' operator returns true if both the left and right operands evaluate to true, otherwise it returns false.
* Or GET [http://customer.ultimo.com/api/v1/object](http://customer.ultimo.com/api/v1/object) /Article?filter=Description like 'PC%' or Description like 'Laptop%' The 'or' operator returns false if both the left and right operands both evaluate to false, otherwise it returns true.
* Not: GET [http://customer.ultimo.com/api/v1/object](http://customer.ultimo.com/api/v1/object) /Article?filter=not Description like '%filter' The 'not' operator returns true if the operand returns false, otherwise it returns false.
* The open and close parenthesis 'and' can be used to control the order of evaluation of a filter expression: GET [http://customer.ultimo.com/api/v1/object](http://customer.ultimo.com/api/v1/object) /Article?filter=\(Description like 'PC%' or Description like 'Laptop%'\) and not \(PurchasePrice gt 1000.0 and PurchasePrice lt 1500.0\)

The following values can be used as operands in a filter expression:

<table>
  <thead>
    <tr>
      <th style="text-align:left">Operand</th>
      <th style="text-align:left">Example value</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">Null</td>
      <td style="text-align:left">
        <p>null</p>
        <p>Note:</p>
        <ul>
          <li>The null value can only be used with the &apos;eq&apos; and &apos;ne&apos;
            operators</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">Boolean</td>
      <td style="text-align:left">
        <p>true</p>
        <p>false</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">Integer</td>
      <td style="text-align:left">-17</td>
    </tr>
    <tr>
      <td style="text-align:left">Decimal</td>
      <td style="text-align:left">1.234</td>
    </tr>
    <tr>
      <td style="text-align:left">String</td>
      <td style="text-align:left">&apos;Hello world&apos;</td>
    </tr>
    <tr>
      <td style="text-align:left">Date</td>
      <td style="text-align:left">2018-03-09</td>
    </tr>
    <tr>
      <td style="text-align:left">DateTimeOffset</td>
      <td style="text-align:left">
        <p>2018-03-09T15:33:51.1355081Z</p>
        <p>2018-03-09T16:33:51.1355081+01:00</p>
        <p>Notes:</p>
        <ul>
          <li>The seconds and fractions are optional</li>
          <li>The plus character must be percent-encoded when used in a real URL</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">TimeOfDay</td>
      <td style="text-align:left">
        <p>16:33:51.1355081</p>
        <p>16:33</p>
        <p>Note:</p>
        <ul>
          <li>The seconds and fractions are optional</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

**Order by**

The 'orderby' query option can be used to specify the order in which a collection is returned. It must contain a comma-separated list of property paths which end on a primitive property. The path can include an 'asc' suffix for ascending order or a 'desc' suffix for descending order. If no suffix is specified the results are returned in ascending order. The results are first ordered on the value of the first property path, then by the second property path and so on.

* Return all articles ordered on ascending description: GET [http://customer.ultimo.com/api/v1/object](http://customer.ultimo.com/api/v1/object) /Article?orderby=Description asc
* Return all articles order on descending description: GET [http://customer.ultimo.com/api/v1/object](http://customer.ultimo.com/api/v1/object) /Article?orderby=Description desc
* Return all articles order by descending description and the by ascending Id GET [http://customer.ultimo.com/api/v1/object](http://customer.ultimo.com/api/v1/object) /Article?orderby=Description desc,Id

**Expand**

The 'expand' query option can be used to specify related entities to include in the response. It must contain a comma-separated list of navigation properties. Each navigation property can be followed by a forward slash and another navigation property to expand a nested relationship. The maximum level of nested relationships that can be expanded is 3.

* For each equipment include all related spareparts and the vendor entity: GET [http://customer.ultimo.com/api/v1/object](http://customer.ultimo.com/api/v1/object) /Equipment?expand=SpareParts,Vendor
* For each building include all related buildingparts and for each buildingpart include all related buildingfloors: GET [http://customer.ultimo.com/api/v1/object](http://customer.ultimo.com/api/v1/object) /Equipment?expand=BuildingParts/BuildingFloors

**Select**

The 'select' query option can be used to specify a subset of properties to include in the response. It must contain a comma-separated list of properties or navigation properties.

* For each equipment return only the description and typenumber: GET [http://customer.ultimo.com/api/v1/object](http://customer.ultimo.com/api/v1/object) /Equipment?select=Description,TypeNumber

The select query option can be combined with the expand query option to include a subset of properties of an expanded entity. The expanded entity must be included in the select query option.

* For each equipment the description is returned and for the expanded vendor only the name is returned: GET [http://customer.ultimo.com/api/v1/object](http://customer.ultimo.com/api/v1/object) /Equipment?select=Description,Vendor/Name&expand=Vendor

**Top**

The 'top' query option can be used to request the number of items in the collection to be included in the result.

* Return the first 10 articles of the collection: GET [http://customer.ultimo.com/api/v1/object/Article?top=10](http://customer.ultimo.com/api/v1/object/Article?top=10)

**Skip**

The 'skip' query option can be used to specify the number of items that are skipped and not included in the result.

* Return the articles starting from the 6th article of the collection: GET [http://customer.ultimo.com/api/v1/object/Article?skip=5](http://customer.ultimo.com/api/v1/object/Article?skip=5)
* Return 10 articles starting from the 21st article from the collection when sorted by description: GET [http://customer.ultimo.com/api/v1/object](http://customer.ultimo.com/api/v1/object) /Article?top=10&skip=20&orderby=Description

**Count**

The 'count' query option can be used to include the count of the matching items of a collection to be included in the result.

* Include the total number of articles in the result: GET [http://customer.ultimo.com/api/v1/object/Article?count=true](http://customer.ultimo.com/api/v1/object/Article?count=true)
* Return the first 10 articles which have a purchase price greater than 500 and include a count of the total number of articles which have a purchase price greater than 500: GET [http://customer.ultimo.com/api/v1/object](http://customer.ultimo.com/api/v1/object) /Article?filter=PurchasePrice gt 500.0&count=true&top=10

**Special characters**

Some special characters can't be directly used in requests. This paragraph explains all limitations and how to work around them.

* You can't use a forward slashes for the Id in the route, because it will be interpreted as a path separator: //ERROR GET [http://customer.ultimo.com/api/v1/object/Building\('01/02'\)](http://customer.ultimo.com/api/v1/object/Building%28)
* Using URL encoding also won't work because WebAPI will URL decode it first and it will result in the same URL: //ERROR GET [http://customer.ultimo.com/api/v1/object/Building\('01%2f02'\)](http://customer.ultimo.com/api/v1/object/Building%28)
* It is possible to use a forward slash in the Id when using a filter: //GOOD GET [http://customer.ultimo.com/api/v1/object/Building?filter=Id](http://customer.ultimo.com/api/v1/object/Building?filter=Id) eq '01/02'

The following special characters also can't be used in the Id, because ASP.NET will give a warning about a potentially dangerous request path value:  
&lt; &gt; \* % & : \ ?  
The detection of these special characters can be disabled but that might be a potential security issue.

* It is possible to use these special characters in a filter when properly URL encoded: //BAD GET [http://customer.ultimo.com/api/v1/object](http://customer.ultimo.com/api/v1/object) /Building?filter=Id eq '&lt;&gt;\*%&:\?' //GOOD GET [http://customer.ultimo.com/api/v1/object](http://customer.ultimo.com/api/v1/object) /Building?filter=Id+eq+'%3C%3E\*%25%26%3A%3F'
* Other special characters can be used in the Id when properly URL encoded: //BAD GET [http://customer.ultimo.com/api/v1/object](http://customer.ultimo.com/api/v1/object) /Building\('$+,;=@ "{}!^~\[\]\`'\) //GOOD GET [http://customer.ultimo.com/api/v1/object](http://customer.ultimo.com/api/v1/object) /Building\('$+,;=@%20%22%7B%7D!%5E~%5B%5D%60'\)

Single quotes inside strings used in a filter expression need to be escaped using two single quotes:

* GET [http://customer.ultimo.com/api/v1/object](http://customer.ultimo.com/api/v1/object) /Building?filter=Description eq 'single''quote'

#### Response format

Ultimo REST API always returns a JSON response.

#### Testing REST

Besides the above described general instructions for requesting data, Ultimo is able to generate key specific documentation. This documentation contains all commands that can be executed in the API connection, including options to try it out. It is specific to the key, as it generates content based on the configuration of the API key. Another possibility to test the REST API is by using Postman.

## Connecting to third party web services

Ultimo Business Integration is able to connect with third party web services. Both import and export connectors offer the possibility to send requests to a webservice url. By default, the request will be send in a fixed Ultimo format. It is however possible to send data in a specified custom \(xml or json\) format. This can be configured by our consultants.

Ultimo Business Integration is also able to connect with web services that require a Expect: 100-continue header in the initial request.

#### Authentication

The following authentication types are supported when connecting to third party web services:

* Basic authentication
* [OAuth2.0](https://oauth.net/2/)
  * Ultimo offers support for grant types Authorization code, Password and Client credentials

## Error handling

Sending a request to Ultimo Business Integration will return an HTTP response status code. A list of all status codes can be found [here](https://en.wikipedia.org/wiki/List_of_HTTP_status_codes). There is one important thing to keep in mind: a 200 OK response does not mean that the request was successfully handled by Ultimo. It only means that the request was received successfully by the API. Whenever a request is successfully received, it will be processed separately afterwards. Details of possible errors will be returned in the response body.

#### Common HTTP response status codes

* 200 OK, Ultimo has received the web request successfully and tried to process it. Details of the processing results are returned in the response body.
* 400 Bad Request, Ultimo is not able to process the request due to an apparent client error. This usually means there is a malformed request syntax.
* 401 Unauthorized, Ultimo is not able to match the specified authentication parameters
* 404 Not found, the used endpoint is probably incorrect
* 500 Internal Server Error, a generic error that can be caused by many different problems. Depending on the failure, the response body might contain detailed information.

#### Error message example

When processing of a request fails, the response body will contain detailed information on what went wrong. These error messages are returned in a fixed format and cannot be customised.

|  |
| :--- |


