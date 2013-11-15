# GET /Joinbox-RESTful-Style-Guide

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

### Methods available on collections

- OPTIONS
- GET
- HEAD
- DELETE
- POST


#### GET

The GET method is used to get an optional filtered, paged set of resources. 

Available request headers
- **Range**: 0 based range index, only available if the options request did return the «Accept-Ranges: resources» header
- **Accept**: The content type that can be accepted, will be «Application/JSON» most of the time
- **Accept-Language**: If this header is omitted the resource will be returned in all available languages, this header may be set multiple times if the resource schould be returned in multiple languages. You may specify fallback languages if the resource is only partial available in the preferred language.
- **X-Select**: CSV list of properties to return, you may obtain a list of available properties using the options request on the collection

The example request below will return 

*Request Headers*
```HTTP
GET /user HTTP/1.1
Host: somehost:12001
Accept: Application/JSON
Accept-Language: de, fr;q=0.9, en;q=0.8
Range: 0-10
X-Select: id, tenant.id, tenant.name, firends.id, friends.name
X-Order: name DESC, firends.name DESC
X-Filter: id=in(3,4), firstName=like(mich%)
```

*Response Headers*
```HTTP
HTTP/1.1 200 OK
Content-Type: Application/JSON
Date: Fri, 15 Nov 2013 12:12:14 GMT
Range: 0-10
```

*Response Body*
```Javascript
[
	{
		  id: 1
		, name: "michael"
		, tenant: {
			  id: 1
			, name: "default tenant"
		}
		, friends: [
			{
				  id: 4
				, name: "fabian"
			}
			, {
				  id: 5 
				, name: "pereg"
			}
		]
	}
	, {
		  id: 2
		, name: "micha"
		, tenant: {
			  id: 4
			, name: "events.ch"
		}
		, friends: []
	}
]
```


### The OPTIONS Method

The options request returns a reponse with the allow header which contains a comma separated list of the methods supported on the collection. The response body may contain a detailed description of the collection.


*Request Headers*
```HTTP
OPTIONS /user HTTP/1.1
Host: somehost:12001
Accept: Application/JSON
```

*Response Headers*
```HTTP
HTTP/1.1 200 OK
Content-Type: Application/JSON
Date: Fri, 15 Nov 2013 12:12:14 GMT
Allow: OPTIONS,GET,POST
Accept-Ranges: resources
X-Accept-Select: id, tenant.id, tenant.name, friends.id, friends.name
X-Accept-Order: *
X-Accept-Group: tenant.id, profile.street, profile.municipality
X-Accept-Filter: id, tenant.id
```

*Response Body*

The data in the response body describes the collection / resource and the subresources of the collection / resource
```Javascript
{
	  allow: ['OPTIONS','GET','POST']
	, properties: {
		id: { 
		  	  primary: 	true
		  	, type: 	'int'
		  	, filters: 	'numbers'
		}
		, tenant: {
			id: { 
			  	  primary: 	true
			  	, type: 	'int'
			  	, filters: 	'numbers'
			}
			, name: {
				  unique: 	true
			  	, type: 	'string'
			  	, filters: 	'strings'
			}
		}
	}
	, filters: {
		  numbers: ['<', '>', '=', '!=', '>=', '<=', 'notNull', 'null', 'in']
		, strings: ['=', '!=', 'like', 'notNull', 'null', 'in']
	}
}
```








### Methods available on resources
- OPTIONS
- GET
- HEAD
- DELETE
- PUT
- PATCH


