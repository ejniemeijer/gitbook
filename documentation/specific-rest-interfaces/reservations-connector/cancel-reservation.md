# Cancel reservation

This workflow allows third parties to cancel an existing reservation for a specific room with specified times.

The following required parameter is used in the request.

| Parameter | Description |
| :--- | :--- |
| UltimoId | The internal Ultimo Id of a reservation for the room. |

### Technical details 

Basic URL \(POST\):

`https://customer.ultimo.com/api/V1/Action/REST_CancelReservation`

Header data:

| ApiKey | Description |
| :--- | :--- |
| ApiKey | Request an API key at \(application manager or consultant\) |
| ApplicationElementId | Fixed value “5d5a10ee-7b14-4dd4-8683-33822ea56f12” |

Body \(example\):

![](../../../.gitbook/assets/12.png)

#### Normal response:

{% hint style="success" %}
Status: 200 OK
{% endhint %}

![](../../../.gitbook/assets/13.png)

#### Response when the API-key is invalid:

{% hint style="danger" %}
Status: 401 Unauthorized
{% endhint %}

![](../../../.gitbook/assets/14.png)



