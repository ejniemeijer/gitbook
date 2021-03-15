---
description: 'TODO: Add the example requests from the third party documentation'
---

# Inserting data

It is possible to change data by sending a request to one of the activated import connectors.

Each object to import should be bundled in a single &lt;object&gt; node. All attributes of an object should be added as properties. The name of a property represents a database field in Ultimo. It is possible to send multiple objects at once. The action to execute per object should be determined when creating an interface.

It is also possible to nest objects, to create a parent/child construction. When nested objects are used, the ${Parent.Id} syntax can be used to refer to the parent. To make sure Ultimo tries to get the value of the parent, attribute Evaluation should be added.

_Example request including a nested object_

```markup
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

_Example response_

Returns the full request or an exception

```markup
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

#### 

