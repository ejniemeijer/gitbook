# Special characters

Some special characters can't be directly used in requests. This paragraph explains all limitations and how to work around them.

* You **can't** use a forward slashes for the Id in the route, because it will be interpreted as a path separator:  ⚠️  //ERROR `GET http://customer.ultimo.com/api/v1/object/Building('01/02')` 
* Using URL encoding also **won't work** because WebAPI will URL decode it first and it will result in the same URL:  ⚠️ //ERROR `GET http://customer.ultimo.com/api/v1/object/Building('01%2f02')` 
* It is possible to use a forward slash in the Id when using a filter:  👍🏻 //GOOD `GET http://customer.ultimo.com/api/v1/object/Building?filter=Id eq '01/02'` 

The following special characters also can't be used in the Id, because ASP.NET will give a warning about a potentially dangerous request path value:    `< > * % & : \ ?`  
  
The detection of these special characters can be disabled but that might be a potential security issue.

* It is possible to use these special characters in a filter when properly URL encoded:  👎🏻 //BAD `GET http://customer.ultimo.com/api/v1/object/Building?filter=Id eq '<>*%&:\?'`  👍🏻 //GOOD `GET http://customer.ultimo.com/api/v1/object/Building?filter=Id+eq+'%3C%3E*%25%26%3A%3F'` 
* Other special characters can be used in the Id when properly URL encoded:  👎🏻 //BAD ``GET http://customer.ultimo.com/api/v1/object/Building('$+,;=@ "{}!^~[]`')``  👍🏻 //GOOD `GET http://customer.ultimo.com/api/v1/object/Building('$+,;=@%20%22%7B%7D!%5E~%5B%5D%60')`

Single quotes inside strings used in a filter expression need to be escaped using two single quotes:

`GET http://customer.ultimo.com/api/v1/object/Building?filter=Description eq 'single''quote'`

