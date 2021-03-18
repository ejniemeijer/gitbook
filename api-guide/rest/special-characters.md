# Special characters

Some special characters can't be directly used in requests. This paragraph explains all limitations and how to work around them.

{% hint style="danger" %}
You can't use a forward slashes for the Id in the route, because it will be interpreted as a path separator:

`GET https://customer.ultimo.net/api/v1/object/Building('01/02')`
{% endhint %}

{% hint style="danger" %}
Using URL encoding also won't work because WebAPI will URL decode it first and it will result in the same URL:

`GET https://customer.ultimo.net/api/v1/object/Building('01%2f02')`  
{% endhint %}

{% hint style="success" %}
It is possible to use a forward slash in the Id when using a filter:

`GET https://customer.ultimo.net/api/v1/object/Building?filter=Id eq '01/02'`
{% endhint %}

The following special characters also can't be used in the Id, because ASP.NET will give a warning about a potentially dangerous request path value:    `< > * % & : \ ?`  
  
The detection of these special characters can be disabled but that might be a potential security issue. It is possible to use these special characters in a filter when properly URL encoded:

{% hint style="danger" %}
`GET https://customer.ultimo.net/api/v1/object/Building?filter=Id eq '<>*%&:\?'`
{% endhint %}

{% hint style="success" %}
`GET https://customer.ultimo.net/api/v1/object/Building?filter=Id+eq+'%3C%3E*%25%26%3A%3F'`
{% endhint %}

Other special characters can be used in the Id when properly URL encoded:

{% hint style="danger" %}
``GET https://customer.ultimo.net/api/v1/object/Building('$+,;=@ "{}!^~[]`')``
{% endhint %}

{% hint style="success" %}
`GET https://customer.ultimo.net/api/v1/object/Building('$+,;=@%20%22%7B%7D!%5E~%5B%5D%60')`
{% endhint %}

Single quotes inside strings used in a filter expression need to be escaped using two single quotes:

`GET https://customer.ultimo.net/api/v1/object/Building?filter=Description eq 'single''quote'`

