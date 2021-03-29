# IoT Connector

The Ultimo IoT connector enables third parties to provide measurements \(i.e. based on sensor data\) to Ultimo. There are three different ways to connect to Ultimo:

1. REST
2. HTTP POST request with an XML in the body
3. SOAP \(XML\)

After importing a measurement, various validations and business rules will be executed in Ultimo.

Ultimo is not suitable for _raw sensor data_, so it is not allowed to directly connect sensors to Ultimo or send unfiltered sensor data to Ultimo. The Ultimo IoT connector is intended to connect with a server or platform that collects and filters / aggregates data from sensors.

![](../../../.gitbook/assets/2%20%282%29.png)

To avoid an overload of sensor data, there is a maximum limit of measurements. When processing measured values, Ultimo will check if there are no more than 3 days with more than 25 measured values in the past 10 days. 

It is also possible to create new measurement points with the REST interface.

### Functional details

Objects or meters can have measurement points. These can be stored on an equipment or a process function. Each measurement should refer to an existing measurement point \(for an equipment or process function\).

![](../../../.gitbook/assets/3%20%282%29.png)

A measurement contains:

<table>
  <thead>
    <tr>
      <th style="text-align:left">Parameter</th>
      <th style="text-align:left">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">EquipmentId or ProcessFunctionId</td>
      <td style="text-align:left">
        <p>Use an <em>EquipmentId</em> or <em>ProcessFunctionId</em> depending on where
          the object is stored (internal Ultimo ID).</p>
        <p>It is recommended to use a Meter, which is an Equipment. This property
          is required.</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">MeasurementPointId</td>
      <td style="text-align:left">The code of the measurement point (int. Ultimo ID).</td>
    </tr>
    <tr>
      <td style="text-align:left">Date</td>
      <td style="text-align:left">
        <p>The date and time of the measurement. This property is required. Allowed
          formats:</p>
        <ul>
          <li>yyyy-MM-dd hh:mm:ss (i.e. 2018-08-21 15:21:59)</li>
          <li>UTC: yyyy-MM-ddThh:mm:ssZ (i.e. 2018-08-20T15:17:59Z)</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">Value or Delta</td>
      <td style="text-align:left">
        <p>Use <em>Value</em> when the measured value should be interpreted as an absolute
          value. Examples: a temperature or the total running hours of a machine.</p>
        <p>Use <em>Delta</em> when the measured value is a difference relative to the
          previous measurement and it is desired to be cumulated in Ultimo. Example:
          the running hours since the last measurement.
          <br />Note: Delta is only available in REST-API.</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">Text</td>
      <td style="text-align:left">Additional information or remarks for the measurement.</td>
    </tr>
  </tbody>
</table>

  
With the REST interface it is possible to create a new measurement point. When the new measurement point is the first measurement point for an equipment or process function, this will be the default measurement point.

A measurement point contains:

| Parameter | Description |
| :--- | :--- |
| EquipmentId or ProcessFunctionId | Use an _EquipmentId_ or _ProcessFunctionId_ depending on where the object is stored \(internal Ultimo ID\). It is recommended to use a Meter, which is an Equipment. This property is required. |
| MeasurementPointId | The new code of the measurement point \(internal Ultimo ID\). This property is required. |
| Description | Additional description for the measurement point. This property is required. |

