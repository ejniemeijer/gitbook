# Third party integration

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


