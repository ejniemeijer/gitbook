# Skip

The 'skip' query option can be used to specify the number of items that are skipped and not included in the result.

* Return the articles starting from the 6th article of the collection:  `GET` [`https://customer.ultimo.net/api/v1/object/Article?skip=5`](https://customer.ultimo.net/api/v1/object/Article?skip=5) 
* Return 10 articles starting from the 21st article from the collection when sorted by description:  `GET` [`https://customer.ultimo.net/api/v1/object`](https://customer.ultimo.net/api/v1/object)`/Article?top=10&skip=20&orderby=Description`

