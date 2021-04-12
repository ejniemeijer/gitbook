# SOAP \(XML\)

#### WSDL:

`https://customer.ultimo.com/webservices/soapconnector.svc?wsdl`

#### The body contains the following information:

| Parameter | Description |
| :--- | :--- |
| importName | Fixed value “IOT-MEASUREMENT” |
| username | Username that has the rights to use the IoT-connector |
| password | Password of the user |
| importData | The measurements XML in a &lt;!\[CDATA\[ ..\]\]&gt; block |

#### Example:

```markup
<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/ xmlns:tem=“http://tempuri.org/>
		<soapenv:Header/>
				<soapenv:Body>
						<tem:ImportData>

						<tem:importName>IOT-MEASUREMENT</tem:importName>

						<tem:userName>userl</tem:userName> 

						<tem:password>PasswOrd!</tem:password>
						<tem:importData>
								<![CDATA[<Measurements>
									<Measurement>
											<EquipmentId>00001</EquipmentId>
											<MeasurementPointId>TEMP</MeasurementPointId>
											<Value>22</Value>
											<Date>2018-08-20715:18:59Z</Date>
											<Text>Some remarks for measurement</Text>
									</Measurement>
									<Measurement>
											<ProcessFunctionId>0001</ProcessFunctionId>
											<MeasurementPointId>RUNHOURS</MeasurementPointId>
											<Value>22</Value>
											<Date>2018-08-21 15:19:59</Date>
											<Text>Some remarks for measurement</Text>
									</Measurement>
								</Measurements>]]> 
			</tem:importData>
		</tem:ImportData>
	</soapenv:Body>
</soapenv:Envelope>
```

The response will contain the imported measurements in the internal Ultimo data structure, including the generated IDs. When a measurement is imported successfully, the attribute _Action_ is “Insert”. Example response:

{% hint style="success" %}
Status: 200 OK
{% endhint %}

```markup
<s:Envelope xmlns:s="http://schemas.xmlsoap.org/soap/envelope/">
	<s:Body>
		<ImportDataResponse xmlns="http://tempuri.org/">
			<ImportDataResult>
				<![CDATA[<?xml version="1.0" encoding="utf-16"?> 
					<Data> 
						<Object Type="EquipmentMeasurementPointValue" Action="Insert"> 
							<Property Name="Id.EquipmentMeasurementPoint.Id.Equipment.Id" Value="00001" /> 
							<Property Name="Id.EquipmentMeasurementPoint.Id.Id" Value="TEMP" /> 
							<Property Name="Id.LineId" Value="00000000000003" /> 
						</Object> 
						<Object Type="ProcessFunctionMeasurementPointValue" Action="Insert"> 
							<Property Name="Id.ProcessFunctionMeasurementPoint.Id.Id" Value="RUNHOURS" /> 
							<Property Name="Id.ProcessFunctionMeasurementPoint.Id.ProcessFunction.Id" Value="0001" /> 
							<Property Name="Id.LineId" Value="00000000000002" /> 
						</Object> 
					</Data>]]>
			</ImportDataResult>
		</ImportDataResponse>
	</s:Body>
</s:Envelope>
```

When the measurement is not accepted by the Ultimo business rules or an error occurs during the processing of the measurement, the _Action_ attribute will have the value “Error”. Example of a response that contains errors:

{% hint style="success" %}
Status: 200 OK
{% endhint %}

```markup
<s:Envelope xmlns:s="http://schemas.xmlsoap.org/soap/envelope/">
	<s:Body>
		<ImportDataResponse xmlns="http://tempuri.org/">
			<ImportDataResult>
				<![CDATA[<?xml version="1.0" encoding="utf-16"?> 
					<Data> 
						<Object Type="EquipmentMeasurementPointValue" Action="Error">
							<Property Name="ErrorMessage" Value="Cannot import entity 'EquipmentMeasurementPointValue' 'ExternalId': '00001-DEFAULT-2018-08-20T15:17:59Z-232' 'Id.EquipmentMeasurementPoint.Id.Equipment.Id': '00001' 'Id.EquipmentMeasurementPoint.Id.Id': 'DEFAULT' 'Date': '2018-08-20T15:17:59Z' 'Value': '232' 'Text': 'Some remarks for measurement' 'Status': 'EquipmentMeasurementPointValueStatus.ToProcess' . Error wile executing workflow 'EquipmentMeasurementPointValue_PostImport'. Message: EquipmentMeasurementPointValue_CheckIfAnotherValueExistsForDateTime Measurement value for measurementpoint DEFAULT of equipment Heftruck 1 type 35  cannot be processed.&#xD;&#xA; Cause: measurement value '(22)' has already been specified for 08/21/2018 03:17." />
							<Property Name="ShortErrorMessage" Value="Error wile executing workflow 'EquipmentMeasurementPointValue_PostImport'. Message: EquipmentMeasurementPointValue_CheckIfAnotherValueExistsForDateTime Measurement value for measurementpoint DEFAULT of equipment Heftruck 1 type 35  cannot be processed.&#xD;&#xA;Cause: measurement value '(22)' has already been specified for 08/21/2018 03:17." />
						</Object>
					</Data>]]>
			</ImportDataResult>
		</ImportDataResponse>
	</s:Body>
</s:Envelope>

```

When the connector cannot be reached at all \(i.e. when username or password is incorrect\), the format of the error message will be as follows:

{% hint style="success" %}
Status: 500 Internal Server Error
{% endhint %}

```markup
<s:Envelope xmlns:s="http://schemas.xmlsoap.org/soap/enveloper>
	<s:Body>
			<s:Fault>
			<faultcode>s:ImportData</faultcode>
			<faultstring xml:lang="nl-NL">Username or password incorrect, login failed.</faultstring>
			<detail>
				<ConnectorErrorMessage xmlns="http://schemas.datacontract.org/(...)>
				<Message>Username or password incorrect, login failed.</Message>
				</ConnectorErrorMessage>
			</detail>
		</s:Fault>
	</s:Body>
</s:Envelope> 
```



