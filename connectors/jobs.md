# Jobs

This REST connector allows third parties to create a job in Ultimo. This functionality is similar to reporting a job in Ultimo. Ultimo acts as a server in this connector. The flow is simple and depicted below:

![](../.gitbook/assets/2%20%281%29.png)

The following parameters are used in the request.

<table>
  <thead>
    <tr>
      <th style="text-align:left">Parameter</th>
      <th style="text-align:left">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">Description</td>
      <td style="text-align:left">The description of the job. This parameter is required.</td>
    </tr>
    <tr>
      <td style="text-align:left">ReportText</td>
      <td style="text-align:left">This is the additional information for a job. The value of this string
        will be set in the text field in section &#x201C;Report&#x201D;. This parameter
        is required.</td>
    </tr>
    <tr>
      <td style="text-align:left">ReportDate</td>
      <td style="text-align:left">
        <p>This is the date on which the job has been reported.</p>
        <p>Allowed formats:</p>
        <ul>
          <li>yyyy-MM-dd hh:mm:ss (i.e. 2018-08-21 15:21:59)</li>
          <li>UTC: yyyy-MM-ddThh:mm:ssZ (i.e. 2018-08- 20T15:17:59Z).</li>
        </ul>
        <p>This property is optional. The default value is the CurrentDateTime.</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">ProcessFunctionId</td>
      <td style="text-align:left">The Id of the entity ProcessFunction. This property is optional. It is
        possible to use ProcessFunction from every context (eg Fleetnumbers).</td>
    </tr>
    <tr>
      <td style="text-align:left">EquipmentId</td>
      <td style="text-align:left">The Id of the entity Equipment. This property is optional. It is possible
        to use Equipment from every context (eg Objects).</td>
    </tr>
    <tr>
      <td style="text-align:left">SpaceId</td>
      <td style="text-align:left">The Id of the entity Space. This property is optional.</td>
    </tr>
    <tr>
      <td style="text-align:left">SiteId</td>
      <td style="text-align:left">The Id of the entity Site. This property is optional.</td>
    </tr>
    <tr>
      <td style="text-align:left">WorkOrderTypeId</td>
      <td style="text-align:left">The Id of the entity WorkOrderType (this is usually the job type of a
        job). This property is optional</td>
    </tr>
    <tr>
      <td style="text-align:left">Context</td>
      <td style="text-align:left">The context for the job. Allowed values: context values/numbers (eg 1)
        or context names (eg JobContext.TD). This property is optional. The default
        value is JobContext.TD.</td>
    </tr>
  </tbody>
</table>

When these parameters are properly filled in, Ultimo will give a HTTP 200 response with the following properties:

| Parameter | Description |
| :--- | :--- |
| JobId | The internal Id of the created job in Ultimo. |

There are a few clarifications for using this connector:

* The created job will always have the status Open \(JobStatus.Created\), as a consequence this created job will be mostly found on the Prepare jobs screens in Ultimo.
* Active Service Level Agreements \(SLA\) can get triggered with this connector. However, the ReportDate can be freely edited in the request, in contrast to the Ultimo application where this field is protected. As a consequence, this means a report date can be set in the past. When an acti_v_e SLA is triggered for say an equipment, it is possible the business logic will place the scheduled start date and the scheduled target date in the past as well. The connector will not give warnings about dates in the past or about expired Service Level Agreements \(for a job\). When the parameter ReportDate has no value or is not used as parameter, the default will be the CurrentDateTime.
* The Site will be filled on the job following a certain order. When the parameter SiteId has a valid value, the Site belonging to this Id will be filled on the job. If SiteId has no value, the Site of Equipment will be filled on the job. If the Equipment does not have a Site \(or if there is no Equipment\), the Site of ProcessFunction will be filled on the job. When a ProcessFunction does not have a Site either \(or there is no ProcessFunction\) , and consequently no Site is filled on the job, the created record might not be visible because of active record authorisation.
* When Equipment has a Department and/or a CostCenter and the application settings ‘Method: Determining the department” and “Method: Determining the cost centre” are set right, these fields will be also be filled on the job.
* If an equipment or a space has an additional building hierarchy \(eg BuildingFloor, BuildingPart , Building\), these fields will also be filled on the created job.

## Technical details

#### Basic URL \(POST\):

`https://customer.ultimo.com/api/V1/Action/REST_ReportJob`

Header data:

| Parameter | Description |
| :--- | :--- |
| ApiKey | Request an API key at \(application manager or consultant\) |
| ApplicationElementId | Fixed value “1ba90cc1-b7f2-452d-8825-101ad7e7a682” |

#### Body \(example\):

```csharp
{
	"Description": "Malfunction in bread line N.PRO.01",
	"ReportText": "The formatting machine is creating ugly shapes",
	"ProcessFunctionld": "N.PRO.01",
	"EquipmentId": "39772",
	"ReportDate": "2020-05-27 10:00:00",
	"Spaceld": "P-42",
	"Siteld": "02",
	"Context": "JobContext.TD",
	"WorkOrderTypeId": "007"
}
```

#### Normal response:

{% hint style="success" %}
Status: 200 OK
{% endhint %}

```csharp
{
	"properties": {
		"JobId": "0005446"
	}
}
```

#### Response when the API-key is invalid:

{% hint style="success" %}
Status: 401 Unauthorized
{% endhint %}

#### Response when the action is not allowed:

{% hint style="success" %}
Status: 400 Bad Request
{% endhint %}

```csharp
{
	"message": "Job cannot be created.\r\nCause: Equipment '397722' cannot be found.",
	"type": 3,
	"code": "3399"
}
```

#### Response when the API-key is invalid:

{% hint style="success" %}
Status: 401 Unauthorized
{% endhint %}

```csharp
{
	"message": "Missing API key",
	"code": "MissingApikey"
}
```

