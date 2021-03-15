# Requesting data

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

