# Overview of a POST request

The following example explains the raw POST request as it is sent to Connector.ashx:

```text
POST  /WebServices/Connector.ashx HTTP/1.1
Host: <>
Content-type: application/x-www-form-urlencoded
Content-length: <<length_of_data>>
Connection: close

Several parameter should be added to the request to make it more specific. Which parameters to use depend on the desired action.
```

<table>
  <thead>
    <tr>
      <th style="text-align:left">Parameter</th>
      <th style="text-align:left">Value</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">Username</td>
      <td style="text-align:left">An existing user in Ultimo (Id) with authorisation to execute connectors</td>
    </tr>
    <tr>
      <td style="text-align:left">Password</td>
      <td style="text-align:left">Password of the above user</td>
    </tr>
    <tr>
      <td style="text-align:left">Action</td>
      <td style="text-align:left">
        <p>&apos;export&apos; when retrieving data from Ultimo</p>
        <p>&apos;import&apos; when changing data in Ultimo</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">ExportName</td>
      <td style="text-align:left">Required when action is &apos;export&apos;. Id of the export connector.</td>
    </tr>
    <tr>
      <td style="text-align:left">ImportName</td>
      <td style="text-align:left">Required when action is &apos;import&apos;. Id of the import connector.</td>
    </tr>
  </tbody>
</table>

