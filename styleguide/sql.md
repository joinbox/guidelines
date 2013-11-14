# SQL Styleguide

This Styleguide defines how SQL statements shoud be formatted.

## Keywords

Keywords should always be UPPERCASE.


```SQL
/* bad */
select * from user;

/* good */
SELECT * FROM user;
```

## Naming

There is **no need** for enclosing named objects with quotation marks. If there is a problem with a named object without quotation marks you should consider using another name for the object.

## Aliases

Aliases must be descriptive, they should not consist out of a single character. 


```SQL
/* bad */
SELECT 
	  u.id
	, up.firstName
	, up.lastName
	, s.name
	, s.municipality
	, s.zip
FROM user u 
LEFT JOIN userProfile up ON up.id_user = u.id 
LEFT JOIN v_street s ON s.id = up.id_street 
LEFT JOIN language l ON l.id = s.id_language 
WHERE l.iso2 = 'en';


/* good */
SELECT 
	  user.id
	, profile.firstName
	, profile.lastName
	, street.name
	, street.municipality
	, street.zip
FROM user
LEFT JOIN userProfile profile ON profile.id_user = user.id 
LEFT JOIN v_street street ON street.id = profile.id_street 
LEFT JOIN language l ON l.id = street.id_language 
WHERE l.iso2 = 'en';

```

## Whitespace

Use soft tabs set to 4 spaces

```SQL
/* bad */
SELECT
∙∙id
FROM user;


/* good */
SELECT
∙∙∙∙id
FROM user;
```

## Commas

Leading commas: Yup.

```SQL
/* bad */
SELECT
	id,
	deleted, 
	created
FROM user;


/* good */
SELECT
	  id
	, deleted
	, created
FROM user;
```


## Linebreaks

Each keyword should begin on a new line -> SELECT, JOIN, LEFT JOIN, OUTER JOIN, WHERE, UNION etc.


```SQL
/* bad */
SELECT u.id, up.firstName, up.lastName, up.birthdate, up.building, up.updated, up.created, s.name, s.municipality, s.zip
FROM user u LEFT JOIN userProfile up ON up.id_user = u.id LEFT JOIN v_street s ON s.id = up.id_street LEFT JOIN
language l ON l.id = s.id_language WHERE l.iso2 = 'en';


/* good */
SELECT 
	  user.id
	, profile.firstName
	, profile.lastName
	, profile.birthdate
	, profile.building
	, profile.updated
	, profile.created
	, street.name
	, street.municipality
	, street.zip
FROM user 
LEFT JOIN userProfile profile ON profile.id_user = user.id 
LEFT JOIN v_street street ON street.id = profile.id_street 
LEFT JOIN language lang ON lang.id = street.id_language 
WHERE lang.iso2 = 'en';
```

## performance

- make sure indexes on the wueries you are filtering on are set.
- try to query first again the good indexes ( id, name etc ) before using indexes / fields like a deleted flag.