# Count

The 'count' query option can be used to include the count of the matching items of a collection to be included in the result.

* Include the total number of articles in the result:  `GET https://customer.ultimo.net/api/v1/object/Article?count=true` 
* Return the first 10 articles which have a purchase price greater than 500 and include a count of the total number of articles which have a purchase price greater than 500:  `GET https://customer.ultimo.net/api/v1/object/Article?filter=PurchasePrice gt 500.0&count=true&top=10`



