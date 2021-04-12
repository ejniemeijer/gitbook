# REST

### Equipment

Create measurement points for an equipment

#### Basic URL \(POST\):

`https://customer.ultimo.com/api/V1/Action/REST_ CreateEquipmentMeasurementPoint`

#### Header data:

| Parameter | Description |
| :--- | :--- |
| ApiKey | Request an API key at \(application manager or consultant\) |
| ApplicationElementId | Fixed value “a90b326b-ff33-421a-a3a8-7d674e37ae22” |

Body \(example\) with measurement point for equipment:

```csharp
{
 "EquipmentId": "00001",
 "MeasurementPointId": "TEMP",
 "Description": "Description for measurement point"
}
```

### Process function

Create measurement points for a process function

#### Basic URL \(POST\):

`https://customer.ultimo.com/api/V1/Action/REST_CreateProcessFunctionMeasurementPoint`

#### Header data:

| Parameter | Description |
| :--- | :--- |
| ApiKey | Request an API key at \(application manager or consultant\) |
| ApplicationElementId | Fixed value “59275fff-2bcb-425c-a933-90513542c212” |

Body \(example\) with measurement point for process function:

```csharp
{
 "ProcessFunctionId": "00001",
 "MeasurementPointId": "TEMP",
 "Description": "Description for measurement point"
}
```

### Equipment measurement point

Measurements for an equipment measurement point

#### Basic URL \(POST\):

`https://customer.ultimo.com/api/V1/Action/REST_EquipmentMeasurementPointValue_Import`

#### Header data:

| Parameter | Description |
| :--- | :--- |
| ApiKey | Request an API key at \(application manager or consultant\) |
| ApplicationElementId | Fixed value “e57ecb14-6c6f-46ff-fbf7-e65c22a3df61” |

#### Body \(example\) with absolute value:

```csharp
{
	"EquipmentId": "00001",
	"MeasurementPointId": "TEMP",
	"Date": "2018-08-20 15:57:59",
	"Value": "22",
	"Text": "Some remarks for measurement"
}
```

#### Body \(example\) with delta:

```csharp
{
	"EquipmentId": "00001",
	"MeasurementPointId": "RUNHOURS",
	"Date": "2018-08-21 15:17:59",
	"Delta": "5",
	"Text": "Some remarks for measurement"
}
```

### Process function measurement point

Measurements for a process function measurement point

#### Basic URL \(POST\):

`https://customer.ultimo.com/api/V1/Action/REST_ProcessFunctionMeasurementPointValue_Import`

#### Header data:

| Parameter | Description |
| :--- | :--- |
| ApiKey | Request an API key at \(application manager or consultant\) |
| ApplicationElementId | Fixed value “7dc5815c-1cce-470f-9656-02113d13e635” |

#### Body \(example\) with absolute value:

```csharp
{
	"ProcessFunctionId": "0001",
	"MeasurementPointId": "RUNHOURS",
	"Date": "2018-08-21 15:17:59",
	"Value": "25482",
	"Text": "Some remarks for measurement"
}
```

#### Body \(example\) with delta:

```csharp
{
	"ProcessFunctionId": "0001",
	"MeasurementPointId": "RUNHOURS",
	"Date": "2018-08-21 15:17:59",
	"Delta": "5",
	"Text": "Some remarks for measurement"
}
```

#### Responses

When the measurement or measurement point is accepted:

{% hint style="success" %}
Status: 200 OK
{% endhint %}

```csharp
{
	"properties": {} 
}
```

#### When the measurement is rejected:

{% hint style="danger" %}
Status: 400 Bad Request
{% endhint %}

```csharp
{
	"message": "Value 5 measured on 21-8-2018 15:17 cannot be processed.\r\nCause: measurement point '000011222 - DEFAULT' was not found.",
	"type": 3, 
	"code": "3095" 
}
```

#### When the measurement point is rejected:

{% hint style="danger" %}
Status: 400 Bad Request
{% endhint %}

```csharp
{
 "message": "Measurement point cannot be added.\r\nCause:equipment '000011222' was not found.",
 "type": 3,
 "code": "3395"
}
```

```csharp
{
 "message": "Measurement point TEMP cannot be added.\r\nCause: equipment 00001 has already been linked to the selected measurement point.",
 "type": 3,
 "code": "0687"
}
```

#### When the API-key is invalid \(for a measurement or a measurement point\):

{% hint style="danger" %}
Status: 401 Unauthorized
{% endhint %}

```csharp
{
	"message": "The API key is invalid",
	"code": "InvalidApiKey"
}
```

#### Other:

{% hint style="danger" %}
Status: 500 Internal Server Error
{% endhint %}

```csharp
{
	"message": "An error has occured", 
	"exceptionType": "System.FormatException", 
	"detailMessage": "Guid should contain 32 digits with 4 dashes (xxxxxxxx-xxxx-xxxx-xxxx-xx)",  
	"type": 1, 
	"stackTrace": " at System.Guid.TryParseGuidMithDashes (....)”,
	"code": "InternalServerError" 
}
```

### 

