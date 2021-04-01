# Get reservations

This workflow allows third parties to get reservations for a specific room during a specified timespan from Ultimo.

The following parameters are used in the request. These are all required.

<table>
  <thead>
    <tr>
      <th style="text-align:left">Parameter</th>
      <th style="text-align:left">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">SpaceId</td>
      <td style="text-align:left">This is the internal Ultimo Id of a room/space (entity).</td>
    </tr>
    <tr>
      <td style="text-align:left">From</td>
      <td style="text-align:left">
        <p>The start time (datetime) of the specified timespan.</p>
        <p>Allowed formats:</p>
        <ul>
          <li>yyyy-MM-dd hh:mm:ss (i.e. 2018-08-21 15:21:59)</li>
          <li>UTC: yyyy-MM-ddThh:mm:ssZ (i.e. 2018-08-20T15:17:59Z)</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">Till</td>
      <td style="text-align:left">The end time (datetime) of the specified timespan. The allowed formats
        are the same as the ones specified above.</td>
    </tr>
  </tbody>
</table>

When these parameters are properly filled in, Ultimo will give a HTTP 200 response with the following properties:

| Parameter  | Description |
| :--- | :--- |
| UltimoId | The internal Ultimo Id of a reservation for the room \(ReservationLine Id\). |
| Description | The description of the reservation for the room. |
| ApplicantId | The Id from the applicant of the reservation. |
| ApplicantName | The name of the applicant of the reservation. |
| StartTime | The start time of the reservation for the room. |
| EndTime | The end time of the reservation for the room. |
| StartTimeIncludingPreparationTime | The start time including the preparation time for the room. |
| EndTimeIncludingCleanupTime | The end time including the cleanup time for the room. |
| Status | The status of the reservation. |

### Technical details 

Basic URL \(POST\):

https://customer.ultimo.com/api/V1/Action/REST\_GetReservationsForSpace

Header data:

| Parameter | Description |
| :--- | :--- |
| ApiKey | Request an API key at \(application manager or consultant\) |
| ApplicationElementId | Fixed value “860ea091-4d76-47d9-a733-c3bb18440dde” |

Body \(example\):

```csharp
{
    "From": "2021-06-13 12:00:00",
    "Till": "2021-06-14 13:00:00",
    "SpaceId": "C-25"
}
```

#### Normal response:

{% hint style="success" %}
Status: 200 OK
{% endhint %}

```csharp
{
    "Properties": {
        "Output": {
            "collection": [
                {
                    "UltimoId": "00000006712",
                    "Description": "Team meeting Front office",
                    "ApplicantId": "ULTIMO",
                    "ApplicantName": "Fasse, J.",
                    "StartTime": "2021-06-14T09:00:00.000000+01:00",
                    "EndTime": "2021-06-14T10:30:00.000000+01:00",
                    "StartTimeIncludingPreperationTime": "2021-06-14T08:50:00.000000+01:00",
                    "EndTime": "2021-06-14T10:40:00.000000+01:00",
                    "Status": "Processed",
                }
             ]   
        }
    }
}
```

#### Response when the API-key is invalid:

{% hint style="danger" %}
Status: 401 Unauthorized
{% endhint %}

```csharp
{
    "message": "Missing API key",
    "code": "MissingApiKey"
}
```

