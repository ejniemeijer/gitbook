# Order by

The 'orderby' query option can be used to specify the order in which a collection is returned. It must contain a comma-separated list of property paths which end on a primitive property. The path can include an 'asc' suffix for ascending order or a 'desc' suffix for descending order. If no suffix is specified the results are returned in ascending order. The results are first ordered on the value of the first property path, then by the second property path and so on.

* Return all articles ordered on ascending description:  `GET https://customer.ultimo.net/api/v1/object/Article?orderby=Description asc` 
* Return all articles order on descending description:  `GET https://customer.ultimo.net/api/v1/object/Article?orderby=Description desc` 
* Return all articles order by descending description and the by ascending Id  `GET https://customer.ultimo.net/api/v1/object/Article?orderby=Description desc,Id` 

