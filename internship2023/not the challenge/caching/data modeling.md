In this page we will discuss how we are planning to make use of key value pairs to store some of the data in the cache.
**How are we going to transform the requests into a key ?**


# Input

## GET
request body should contain
- **resource_id**
- **type of resource ?**; could be event, slotset, slot or promotion. Should we save what type of model does the pair hold ?

### Key
The key could hold the **resource_id** just as is, since it's already a random UUID. I think it's very unlikely to have ID collision between all data

- **edge cases** what if
	1. there is no key? since we omit the **id** field when we are requesting to fetch all resources of a model. [[#handle empty key]]
	2. we have more than one field in the request body
	3. we have many users that don't get served the same cached data [[#handle many users]]

### Value
The value of the pair should contain the result of the GET request of the key obviously
Also
I think it should contain the TTL (time-to-live) field to invalidate the cached resource
we might also want to store resource type along with it

#### handle empty key
Since there is only one cause for this edge case which is a GET request without a resource_id, we can hard code in a way that when there isn't a resource_id in the request body we instead fill that with **allEventResources** for example. Pay attention to the case where we have many users, it also applies here. And also we need to don't need the model type here because we can extravt it from the uri.

#### handle many users
Incase of having many users, we need to store the user_id in the key or value of the cached key-value pair. That way when the cache receives the get request it knows if the user has cached data or not.

I actually even think we should partition all cached data on the user_id, the value of the pair would be a map containing other keys specific to that user. 
OR 
Add a user_id prefix to the key for the search to be instant and that way the value won't be as big as before.

## POST
> when the cache server receives a POST request
> it extracts the resource id + the payload, performs some data modeling transformations [[#cache data transformations]] and adds it to the cache


## cache data transformations
having receiving a resource_id and a payload + user_id that could be extracted from the cookie or sent with the payload
```
requestCtx.request.uri.path.toString() + "?" + requestCtx.request.uri.rawQueryString.getOrElse("")
```
- key : claims + "/model" + "?" + "resource_id" --> unique value
	- example values: claims/event?id=resource_id
- value : 
	- it's much more efficient to keep it as a JsValue to just send it back immediately
