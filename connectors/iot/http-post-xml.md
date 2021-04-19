# HTTP POST \(XML\)

#### Basic URL \(POST\):

`https://customer.ultimo.com/webservices/connector.ashx`

#### Parameters:

| Parameter | Description |
| :--- | :--- |
| username | Username that has the rights to use the IoT-connector |
| password | Password of the user |
| action | Fixed value “import” |
| importName | Fixed value “IOT-MEASUREMENT” |

Note: escape a character when using special characters in the URL.

#### Example:

`https://customer.ultimo.com/webservices/connector.ashx?userName=user1&password=Passw0rd%21&action=import&importName=IOT-MEASUREMENT`

The body can contain multiple measurements:

```csharp
<?xml version="1.0" encoding="utf-8"?>
<Measurements>
	<Measurement>
		<EquipmentId>00001</EquipmentId>
		<MeasurementPointId>TEMP</MeasurementPointId>
		<Value>22</Value>
		<Date>2018-08-20T15:17:59Z</Date>
		<Text>Some remarks for measurement</Text>
	</Measurement>
	<Measurement>
		<ProcessFunctionId>0001</ProcessFunctionId>
		<MeasurementPointId>RUNHOURS</MeasurementPointId>
		<Value>22</Value>
		<Date>2018-08-21 15:17:59</Date>
		<Text>Some remarks for measurement</Text>
	</Measurement>
</Measurements>
```

The response will contain the imported measurements in the internal Ultimo data structure, including the generated IDs. When a measurement is imported successfully, the attribute _Action_ is “Insert”. Example response:

{% hint style="success" %}
Status: 200 OK
{% endhint %}

```cpp
<?xml version="1.0" encoding="utf-8"?>
<Data>
	<Object Type="EquipmentMeasurementPointValue" Action="Insert">
		<Property Name="Id.EquipmentMeasurementPoint.Id.Equipment.Id" Value="00001" />
		<Property Name="Id.EquipmentMeasurementPoint.Id.Id" Value="TEMP" />
		<Property Name="Id.LineId" Value="00000000000001" />
	</Object>
	<Object Type="ProcessFunctionMeasurementPointValue" Action="Insert">
		<Property Name="Id.ProcessFunctionMeasurementPoint.Id.Id" Value="RUNHOURS" />
		<Property Name="Id.ProcessFunctionMeasurementPoint.Id.ProcessFunction.Id" Value="0001" />
		<Property Name="Id.LineId" Value="00000000000001" />
	</Object>
</Data>
```

When the measurement is not accepted by the Ultimo business rules or an error occurs during the processing of the measurement, the _Action_ attribute will have the value “Error”. Example of a response that contains errors:

{% hint style="success" %}
Status: 200 OK
{% endhint %}

```csharp
<?xml version="1.0" encoding="utf-8"?>
<Data>
	<Object Type="EquipmentMeasurementPointValue" Action="Error">
		<Property Name="ErrorMessage" Value="Cannot import entity 'EquipmentMeasurementPointValue' 'ExternalId': '00001-DEFAULT-2018-08-20T15:17:59Z-232' 'Id.EquipmentMeasurementPoint.Id.Equipment.Id': '00001' 'Id.EquipmentMeasurementPoint.Id.Id': 'DEFAULT' 'Date': '2018-08-20T15:17:59Z' 'Value': '232' 'Text': 'Some remarks for measurement' 'Status': 'EquipmentMeasurementPointValueStatus.ToProcess' . Error while executing workflow 'EquipmentMeasurementPointValue_PostImport'. Message: EquipmentMeasurementPointValue_CheckIfAnotherValueExistsForDateTime Measurement value for measurementpoint DEFAULT of equipment Heftruck 1 type 35  cannot be processed.&#xD;&#xA; Cause: measurement value '(22)' has already been specified for 08/21/2018 03:17." />
		<Property Name="ShortErrorMessage" Value="Error while executing workflow 'EquipmentMeasurementPointValue_PostImport'. Message: EquipmentMeasurementPointValue_CheckIfAnotherValueExistsForDateTime Measurement value for measurementpoint DEFAULT of equipment Heftruck 1 type 35  cannot be processed.&#xD;&#xA;Cause: measurement value '(22)' has already been specified for 08/21/2018 03:17." />
	</Object>
</Data>

```

When the connector cannot be reached at all \(i.e. when username or password is incorrect\), the format of the error message will be as follows:

{% hint style="success" %}
Status: 500 Internal Server Error
{% endhint %}

```markup
Ultimo.Framework.Core.Tools.GeneralException: Username or password not correct, login failed. 
	at Ultimo.Web.Core.Authentication.ConnectorLogOn(String userName, String password) 
	at Ultimo.Web.SiteRoot.WebServices.Connector.System.Web.IHttpNandler.ProcessRequest(HttpContext context) 
```

### 

