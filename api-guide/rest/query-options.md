# Query options

The query options are based on the 'OData version 4' standard query options.

### Filters

The 'filter' query option can be used to filter the collection of entities. The following operators can be used to filter:

| Note |
| :--- |
| The spaces around the operators used in the following examples must be percent-encoded as %20 when used in a real URL. |

* Equals:    `GET https://customer.ultimo.net/api/v1/object/Article?filter=Description eq 'Luchtfilter'`    The 'eq' operator returns true if the left operand is equal to the right operand, otherwise it returns false.  
* Not Equals:    `GET https://customer.ultimo.net/api/1/object/Article?filter=Description ne 'Luchtfilter'`    The 'ne' operator returns true if the left operand is not equal to the right operand, otherwise it returns false.  
* Greater Than:    `GET https://customer.ultimo.net/api/V1/object/Article?filter=PurchasePrice gt 500.0`    The 'gt' operator returns true if the left operand is greater than the right operand, otherwise it returns false.  
* Greater Than or Equal:    `GET https://customer.ultimo.net/api/v1/object/Article?filter=PurchasePrice ge 500.0`    The 'ge' operator returns true if the left operand is greater than or equal to the right operand, otherwise it returns false.  
* Less Than:    `GET https://customer.ultimo.net/api/v1/object/Article?filter=PurchasePrice lt 1.0`  The   'lt' operator returns true if the left operand is less than the right operand, otherwise it returns false.  
* Less Than or Equal:    `GET https://customer.ultimo.net/api/v1/object/Article?filter=PurchasePrice le 1.0`    The 'le' operator returns true if the left operand is less than or equal to the right operand, otherwise it returns false.  
* Like:    `GET https://customer.ultimo.net/api/v1/object/Article?filter=Description like 'Lucht%'`    The 'like' operator returns true if the left operand matches the pattern of the right operand, otherwise it returns false. The % wildcard matches any string of zero or more characters. The \_ wildcard matches any single character.  
* And:   `GET https://customer.ultimo.net/api/v1/object/Article?filter=PurchasePrice gt 1000.0 and PurchasePrice lt 1500.0`    The 'and' operator returns true if both the left and right operands evaluate to true, otherwise it returns false.  
* Or:    `GET https://customer.ultimo.net/api/v1/object/Article?filter=Description like 'PC%' or Description like 'Laptop%'`    The 'or' operator returns false if both the left and right operands both evaluate to false, otherwise it returns true.  
* Not:    `GET https://customer.ultimo.net/api/v1/object/Article?filter=not Description like '%filter'`    The 'not' operator returns true if the operand returns false, otherwise it returns false.  
* The open and close parenthesis 'and' can be used to control the order of evaluation of a filter expression:    `GET https://customer.ultimo.net/api/v1/object/Article?filter=(Description like 'PC%' or Description like 'Laptop%') and not (PurchasePrice gt 1000.0 and PurchasePrice lt 1500.0)` 

The following values can be used as operands in a filter expression:

<table>
  <thead>
    <tr>
      <th style="text-align:left">Operand</th>
      <th style="text-align:left">Example value</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">Null</td>
      <td style="text-align:left">
        <p>null</p>
        <p>Note:</p>
        <ul>
          <li>The null value can only be used with the &apos;eq&apos; and &apos;ne&apos;
            operators</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">Boolean</td>
      <td style="text-align:left">
        <p>true</p>
        <p>false</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">Integer</td>
      <td style="text-align:left">-17</td>
    </tr>
    <tr>
      <td style="text-align:left">Decimal</td>
      <td style="text-align:left">1.234</td>
    </tr>
    <tr>
      <td style="text-align:left">String</td>
      <td style="text-align:left">&apos;Hello world&apos;</td>
    </tr>
    <tr>
      <td style="text-align:left">Date</td>
      <td style="text-align:left">2018-03-09</td>
    </tr>
    <tr>
      <td style="text-align:left">DateTimeOffset</td>
      <td style="text-align:left">
        <p>2018-03-09T15:33:51.1355081Z</p>
        <p>2018-03-09T16:33:51.1355081+01:00</p>
        <p>Notes:</p>
        <ul>
          <li>The seconds and fractions are optional</li>
          <li>The plus character must be percent-encoded when used in a real URL</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">TimeOfDay</td>
      <td style="text-align:left">
        <p>16:33:51.1355081</p>
        <p>16:33</p>
        <p>Note:</p>
        <ul>
          <li>The seconds and fractions are optional</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

### Order by

The 'orderby' query option can be used to specify the order in which a collection is returned. It must contain a comma-separated list of property paths which end on a primitive property. The path can include an 'asc' suffix for ascending order or a 'desc' suffix for descending order. If no suffix is specified the results are returned in ascending order. The results are first ordered on the value of the first property path, then by the second property path and so on.

* Return all articles ordered on ascending description:    `GET https://customer.ultimo.net/api/v1/object/Article?orderby=Description asc`  
* Return all articles order on descending description:    `GET https://customer.ultimo.net/api/v1/object/Article?orderby=Description desc`  
* Return all articles order by descending description and the by ascending Id    `GET https://customer.ultimo.net/api/v1/object/Article?orderby=Description desc,Id` 

### Expand

The 'expand' query option can be used to specify related entities to include in the response. It must contain a comma-separated list of navigation properties. Each navigation property can be followed by a forward slash and another navigation property to expand a nested relationship. The maximum level of nested relationships that can be expanded is 3.

* For each equipment include all related spareparts and the vendor entity:    `GET https://customer.ultimo.net/api/v1/object/Equipment?expand=SpareParts,Vendor`  
* For each building include all related buildingparts and for each buildingpart include all related buildingfloors:    `GET https://customer.ultimo.net/api/v1/object/Equipment?expand=BuildingParts/BuildingFloors`

### Select

The 'select' query option can be used to specify a subset of properties to include in the response. It must contain a comma-separated list of properties or navigation properties.

* For each equipment return only the description and typenumber:    `GET https://customer.ultimo.net/api/v1/object/Equipment?select=Description,TypeNumber`

The select query option can be combined with the expand query option to include a subset of properties of an expanded entity. The expanded entity must be included in the select query option.

* For each equipment the description is returned and for the expanded vendor only the name is returned:    `GET https://customer.ultimo.net/api/v1/object/Equipment?select=Description,Vendor/Name&expand=Vendor`

### Top

The 'top' query option can be used to request the number of items in the collection to be included in the result.

* Return the first 10 articles of the collection:    `GET https://customer.ultimo.net/api/v1/object/Article?top=10`

### Skip

The 'skip' query option can be used to specify the number of items that are skipped and not included in the result.

* Return the articles starting from the 6th article of the collection:    `GET` [`https://customer.ultimo.net/api/v1/object/Article?skip=5`](https://customer.ultimo.net/api/v1/object/Article?skip=5)  
* Return 10 articles starting from the 21st article from the collection when sorted by description:    `GET` [`https://customer.ultimo.net/api/v1/object`](https://customer.ultimo.net/api/v1/object)`/Article?top=10&skip=20&orderby=Description`

### Count

The 'count' query option can be used to include the count of the matching items of a collection to be included in the result.

* Include the total number of articles in the result:    `GET https://customer.ultimo.net/api/v1/object/Article?count=true`  
* Return the first 10 articles which have a purchase price greater than 500 and include a count of the total number of articles which have a purchase price greater than 500:    `GET https://customer.ultimo.net/api/v1/object/Article?filter=PurchasePrice gt 500.0&count=true&top=10`

