# Select

The 'select' query option can be used to specify a subset of properties to include in the response. It must contain a comma-separated list of properties or navigation properties.

* For each equipment return only the description and typenumber:  `GET https://customer.ultimo.net/api/v1/object/Equipment?select=Description,TypeNumber`

The select query option can be combined with the expand query option to include a subset of properties of an expanded entity. The expanded entity must be included in the select query option.

* For each equipment the description is returned and for the expanded vendor only the name is returned:  `GET https://customer.ultimo.net/api/v1/object/Equipment?select=Description,Vendor/Name&expand=Vendor`

