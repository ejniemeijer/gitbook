---
description: 'TODO: Add the example requests from the third party documentation'
---

# TODO: Inserting data

It is possible to change data by sending a request to one of the activated import connectors.

Each object to import should be bundled in a single &lt;object&gt; node. All attributes of an object should be added as properties. The name of a property represents a database field in Ultimo. It is possible to send multiple objects at once. The action to execute per object should be determined when creating an interface.

It is also possible to nest objects, to create a parent/child construction. When nested objects are used, the ${Parent.Id} syntax can be used to refer to the parent. To make sure Ultimo tries to get the value of the parent, attribute Evaluation should be added.

_Example request including a nested object_

|  |
| :--- |


_Example response_

Returns the full request or an exception

|  |
| :--- |


#### 

