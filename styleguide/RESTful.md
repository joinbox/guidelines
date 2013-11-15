# GET /Joinbox/RESTful/Style/Guide

This styleguide describes how RESTful Services should be designed. The target is to make the services available to other services and apps as simple as possible. Because of this it is very important all services use the same headers, parameters, encodings, methods and wokrflows.

Much of the functionality will be handled by the ee framework.


## Naming 

- **Collection**: A collection is a collection of resources like users, events or venues
- **Resource**: A resource is a single item of a collection, like one event or one user
- **Request**: Method: A HTTP request method: GET, POST, DELETE, OPTIONS, HEAD, PUT, PATCH
- **Headers**: Stadard HTTP request / response headers 
- **Body**: HTTP request / response body
- **Status**: HTTP response status code like 200, 400 or 500



## URLs

Every entity is a collection and can be accessed through an URL. The URL consits of a descriptive name of the entity. You should always use the singular form for a collection name.

**Bad**
- /Users
- /EventboosterUsers

**Good**
- /user


Resources must be made available throught an URL consisting of the collection, a slash and a unique resource identifier.

**Bad**
- /Users/byId/1
- /User1

**Good**
- /user/1
- /user/michael%40joinbox.com


A resource may have as many subresources / subcollections as required, the URL for the subresource must follow the same rules as the collection / resource it depends on. Since subresources are basically mappings they can be made avialable as subresource or a separate resource or as both.

**Bad**
- /user/1/comment
- /user/1/usercomments/3

**Good**
- /user/1/comment
- /user/1/comment/34


## Methods

Methods available on collections
- OPTIONS
- GET
- HEAD
- DELETE
- POST

Methods available on resources
- OPTIONS
- GET
- HEAD
- DELETE
- PUT
- PATCH


