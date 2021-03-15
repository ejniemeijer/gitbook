---
description: 'TODO: Add the example requests from the third party documentation'
---

# Requesting data

It is possible to retrieve data by sending a request to one of the activated export connectors. Without sending requestData, all data from the connector will be returned. It is possible to add filters to a request to retrieve specific information from the connector. Filters can be applied by adding them in the requestData node within a CDATA block.

_Example request_

```text
<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/" xmlns:tem="http://tempuri.org/">
    <soapenv:Header/>
    <soapenv:Body>
        <tem:ExportData>
            <tem:exportName>exportconnector</tem:exportName>
            <tem:userName>user</tem:userName>
            <tem:password>password</tem:password>
            <tem:requestData>
                <![CDATA[<Data>
                <RequestInfo>
                    <Property Name="Code" Value="[FilterValue]" />
                </RequestInfo>
            </Data>]]>
            </tem:requestData>
        </tem:ExportData>
    </soapenv:Body>
</soapenv:Envelope>
```

_Example response_

Returns the full request or an exception

```text
<s:Envelope xmlns:s="http://schemas.xmlsoap.org/soap/envelope/">
    <s:Body>
        <ExportDataResponse xmlns="http://tempuri.org/">
            <ExportDataResult>
                <![CDATA[<Data>
                    <Object Type="EntityName" Action="InsertOrUpdate">
                        <Property Name="Id" Value="[FilterValue]" />
                        <Property Name="Description" Value="Object 1" />
                    </Object>
                </Data>]]>
            </ExportDataResult>
        </ExportDataResponse>
    </s:Body>
</s:Envelope>
```

The request and response are expected and returned in a fixed Ultimo format. With XSL transformations, it is possible to transform the request \(to the Ultimo format\) and response \(to the desired custom format\) when using XML as an output result. This is optional and can only be done by our consultants. Output can also be returned in a default CSV or JSON format.

