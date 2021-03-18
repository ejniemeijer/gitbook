# Expand

The 'expand' query option can be used to specify related entities to include in the response. It must contain a comma-separated list of navigation properties. Each navigation property can be followed by a forward slash and another navigation property to expand a nested relationship. The maximum level of nested relationships that can be expanded is 3.

* For each equipment include all related spareparts and the vendor entity:  `GET https://customer.ultimo.net/api/v1/object/Equipment?expand=SpareParts,Vendor` 
* For each building include all related buildingparts and for each buildingpart include all related buildingfloors:  `GET https://customer.ultimo.net/api/v1/object/Equipment?expand=BuildingParts/BuildingFloors`



