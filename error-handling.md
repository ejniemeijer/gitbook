# Error handling

Sending a request to Ultimo Business Integration will return an HTTP response status code. A list of all status codes can be found [here](https://en.wikipedia.org/wiki/List_of_HTTP_status_codes). There is one important thing to keep in mind: a 200 OK response does not mean that the request was successfully handled by Ultimo. It only means that the request was received successfully by the API. Whenever a request is successfully received, it will be processed separately afterwards. Details of possible errors will be returned in the response body.

#### Common HTTP response status codes

* `200 OK`, Ultimo has received the web request successfully and tried to process it. Details of the processing results are returned in the response body.
* `400 Bad Request`, Ultimo is not able to process the request due to an apparent client error. This usually means there is a malformed request syntax.
* `401 Unauthorized`, Ultimo is not able to match the specified authentication parameters
* `404 Not found`, the used endpoint is probably incorrect
* `500 Internal Server Error`, a generic error that can be caused by many different problems. Depending on the failure, the response body might contain detailed information.

#### Error message example

When processing of a request fails, the response body will contain detailed information on what went wrong. These error messages are returned in a fixed format and cannot be customised.

Example:

```text
<?xml version="1.0" encoding="utf-8"?>
<Data>
    <Object Type="Equipment" Action="Error">
        <Property Name="ErrorMessage" Value="Cannot import entity 'Equipment' 'Id': 'EQM01' 'Description': 'Parent object' . Import of child object failed" />
        <Property Name="ShortErrorMessage" Value="Import of child object failed" />
    </Object>
    <Object Type="Equipment" Action="Error">
        <Property Name="ErrorMessage" Value="Cannot import entity 'Equipment' 'Description': 'Child object' 'Department.Id': 'UNKNOWN' . Unable to find required relation of type Department for entity Equipment on property Department with value UNKNOWN" />
        <Property Name="ShortErrorMessage" Value="Unable to find required relation of type Department for entity Equipment on property Department with value UNKNOWN" />
    </Object>
</Data>
```



