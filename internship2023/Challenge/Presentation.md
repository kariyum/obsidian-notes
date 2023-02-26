Greetings everyone, I’m karim ben amara backend engineering intern at cognira. I'm here to showcase my work on the challenge. 
In the backend-team we were given two weeks to develop a basic web server that serves data through Restful APIs. The project was full of challenges that we had to overcome like compatibility issues; integrating swagger documentation for example.

Let’s divide this challenge into some small objectives. 
*The most important part is the APIs; they should operate as expected*
At first let's fully understand what the APIs should serve to define a suitable and performant cassandra data model. 
Given 4 csv files : product location sales and calendar each of which has its own schema and could be used by the APIs. 

We have 3 APIs to develop let’s go through them one by one. 

**API 1 :**
Returns sales aggregated to the (sku, store) level with other product attributes. Sku is the unique identifier of a product.
This should be a GET endpoint given the (sku, store) as query parameters the api returns the sum of sales of that specific product in the given store along with other product attributes.

GET("api/sales?sku=sku1&str=store1") is a logical restful api
Performing sales aggregation for every GET request is not wise and efficient. Solving this problem is as simple as preparing the aggregated sales table before hand in cassandra. So the server will query the new sales aggregated table for a faster response.

**API 2:**
Returns calendar information between Today - 2 years and open in the future
If the today is 02/02/2023 the api should return calendar information from 02/02/2021 give or take a day or two counting for february.

API 2 endpoint should look like this
GET("api/calendar")
The primary key of the calendar table is Datenum having values like this 20210202 which is year/month/day zipped or concatenated together. Performing a filter query on this table with cassandra would need a "ALLOW FILTERING" option which it shouldn't be. This problem is solved by giving a shared value (id='1' for example) between all records and set it as a partition key and set the datenum column as a clustering key. The request should filter on id='1' and datenum >= currentdate-2years. This query doesn't need the "Allow filtering" option and is pretty fast.

**API 3:**
Search for a specific item. The search could be on sku, itemlabel or store. Meaning that the api should accept only one of them.
Query examples:
GET("api/item?sku='sku1'") -> returns json object containing products having sku='sku1'
GET("api/item?itemlabel='itemlabel1'") -> returns json object containing products where itemlabel='itemlabel1'.
GET("api/item?str='store1'") -> returns json object containing str='store1'

**API 4:** /api/register
For authentication, we used JWT json web token to authenticate the user and give it access to the resources; API endpoints. 

User sign ups require minimal credentials like user email, password and a password confirmation.
The server checks if the user figures in the system if not it saves it in the cassandra database under the user table. The schema of the user table is (email: String, password: String) and email column is set as the partition key.

POST("api/register")
HttpEntity({
"email" : "example@gmail.com",
"password" : "password",
"confirmPassword" : "password"
})

**API 5:** /api/login
For logging in the system, the user should provide the email and password composed in a json object.
The system will then respond with a json authentication token if the request credentials are correct and figures in the system. Else it will tell you your correct password just in case you didn't note it down.

POST("api/login")
HttpEntity({
"email" : "example@gmail.com",
"password" : "password"
})

NOTE: all other listed APIs needs a jwt authentication. HeaderValue("Authentication") Bearer Token.

## Deployment
As for deployment there are two options; either with docker-compose or with kubernetes

**Docker-compose** 
Running `docker-compose build` in the project folder will build the **akka-server** image.
Running `docker-compose up` will create two containers:
1. The **cassandra** container that will be in the running state
2. The **akka-server** container will be in the idle state waiting for cassandra to finish loading and become healthy.
Once cassandra container finishes booting, expect that the **akka-server** container will at first check if cassandra has the tables that the server will work with and are not empty, if not the container will load missing tables. 
At this step, cassandra should have all tables full of data. The server will right then boot up and starts accepting requests.
There aren't any authorization requirements on the **api/api-docs** endpoint. This will redirect you to the swagger UI with the corresponding json object and will then show you the documentation of each api exposed on the server.

```json
HttpEntity({
"email": "should.work@example.com",
"password": "password"
})
```

