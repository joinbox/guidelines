# GET /Joinbox/RESTful/Style/Guide

This styleguide describes how RESTful Services should be designed at joinbox. The target is to be able to make most services as simple available to other services and apps as possible. Because of this it is very important all services use the same headers, parameters, encodings, methods and wokrflows.

Many of the specifications described below will be handled by parts of the ee framework.



## Naming 

- **Collection**: A collection is a collection of resources like users, events or venues
- **Resource**: A resource is a single item of a collection, like one event or one user
- **Request**: Method: A HTTP request method: GET, POST, DELETE, OPTIONS, HEAD, PUT, PATCH
- **Headers**: Stadard HTTP request / response headers 
- **Body**: HTTP request / response body
- **Status**: HTTP response status code like 200, 400 or 500



## URLs

Every entity is a collection and can be access through an URL. The URL consits of a descriptive name of the entity. You shoud use the singular form.

**Bad**
- /Users
- /EventboosterUsers

**Good**
- /user






