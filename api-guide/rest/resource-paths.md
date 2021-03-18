# Resource paths

The next examples explain the different uses of resource paths:

* Retrieve a list of entities by using an entity set:   ~~`GET https://customer.ultimo.net/api/v1/object/Equipment/`~~ 
* Retrieve a list of custom entities:  `GET https://customer.ultimo.net/api/V1/Object/_Entity/` 
* Retrieve a single entity by using an entity key:  `GET https://customer.ultimo.net/api/v1/object/Equipment('00001')` 
* Retrieve a single entity with a composite key by using multiple key properties:  `GET https://customer.ultimo.net/api/v1/object/EquipmentSparePart(Equipment='00001',Article='0191')` 
* The resource path can be nested so it is possible to retrieve a single entity by following a navigation property from a single entity to another single entity:  `GET https://customer.ultimo.net/api/v1/object/Equipment('00001')/Department` 
* Or by following a navigation property from a single entity to a collection of entities:  `GET https://customer.ultimo.net/api/v1/object/Department('01')/Jobs` 
* The path segments can be composed recursively:  `GET https://customer.ultimo.net/api/v1/object/Department('01')/Jobs('0000002')/Equipments` 
* When retrieving an entity with multiple key properties that includes a navigation property of the related entity the key properties can be omitted:  `GET https://customer.ultimo.net/api/v1/object/Building('0001')/BuildingParts(Building='0001',Id='001')/BuildingFloor` 
* The previous example can be shortened to:  `GET https://customer.ultimo.net/api/v1/object/Building('0001')/BuildingParts('001')/BuildingFloor`



