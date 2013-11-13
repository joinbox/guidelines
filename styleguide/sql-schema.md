# SQL Schema Styleguide

This Styleguide defines how tables should be defined.



## Naming

Table & column names must be defined singular.

```SQL
/* bad */
CREATE TABLE users (
	  id INT(11) NOT NULL AUTO_INCREMENT
	, name VARCHAR(100) NOT NULL
	, PRIMARY KEY (id)
)

/* good */
CREATE TABLE user (
	  id INT(11) NOT NULL AUTO_INCREMENT
	, name VARCHAR(100) NOT NULL
	, PRIMARY KEY (id)
)
```

Table & column names must be camelCase except for names referencing another entity. Tablenames of mapping tables must be defined as follows: «tableX_tableY». Columns referncing a column in another table must be defined as follows: «targetTableColumnName_targetTableTableName».


```SQL
/* a user has n email addresess, 1:n */

/* bad */
CREATE TABLE user_e-mails (
	  id INT(11) NOT NULL AUTO_INCREMENT
	, userId INT(11) NOT NULL
	, email VARCHAR(200) NOT NULL
	, PRIMARY KEY (id)
	, KEY userId(userId)
	, CONSTRAINT fk_user FOREIGN KEY (userId) REFERENCES user(id)
)

/* good */
CREATE TABLE userEmail (
	  id INT(11) NOT NULL AUTO_INCREMENT
	, id_user INT(11) NOT NULL
	, email VARCHAR(200) NOT NULL
	, PRIMARY KEY (id)
	, KEY id_user(id_user)
	, CONSTRAINT fk_user FOREIGN KEY (id_user) REFERENCES user(id)
)
```


```SQL
/* a user has n images which are stored in a central collection, n:n */
CREATE TABLE image (
	  id INT(11) NOT NULL AUTO_INCREMENT
	, path VARCHAR(200) NOT NULL
	, PRIMARY KEY (id)
)
```


```SQL
/* bad */
CREATE TABLE userImages (
	  userId INT(11) NOT NULL
	, image_id INT(11) NOT NULL
	, PRIMARY KEY (userId,image_id)
	, CONSTRAINT fk_user FOREIGN KEY (userId) REFERENCES user(id)
	, CONSTRAINT fk_image FOREIGN KEY (image_id) REFERENCES image(id)
)

/* good */
CREATE TABLE user_images (
	  id_user INT(11) NOT NULL
	, id_image INT(11) NOT NULL
	, PRIMARY KEY (id_user,id_image)
	, CONSTRAINT fk_user FOREIGN KEY (id_user) REFERENCES user(id)
	, CONSTRAINT fk_image FOREIGN KEY (id_image) REFERENCES image(id)
)
```

## Normalization

It's important that all data is stored normalized.


```SQL
/* bad */
CREATE TABLE userAddress (
	  id INT(11) NOT NULL AUTO_INCREMENT
	, id_user INT(11) NOT NULL
	, street VARCHAR(200) NOT NULL
	, building VARCHAR(100) NOT NULL
	, zip VARCHAR(100) NOT NULL
	, municipality VARCHAR(150) NOT NULL
	, PRIMARY KEY (id)
	, KEY id_user(id_user)
	, CONSTRAINT fk_user FOREIGN KEY (id_user) REFERENCES user(id)
)

/* good */
CREATE TABLE country (
	  id INT(11) NOT NULL AUTO_INCREMENT
	, name VARCHAR(200) NOT NULL
	, PRIMARY KEY (id)
)

CREATE TABLE municipality (
	  id INT(11) NOT NULL AUTO_INCREMENT
	, id_country INT(11) NOT NULL
	, name VARCHAR(200) NOT NULL
	, PRIMARY KEY (id)
	, KEY id_country(id_country)
	, CONSTRAINT fk_country FOREIGN KEY (id_country) REFERENCES country(id)
)

CREATE TABLE street (
	  id INT(11) NOT NULL AUTO_INCREMENT
	, id_municipality INT(11) NOT NULL
	, name VARCHAR(200) NOT NULL
	, PRIMARY KEY (id)
	, KEY id_municipality(id_municipality)
	, CONSTRAINT fk_municipality FOREIGN KEY (id_municipality) REFERENCES municipality(id)
)

CREATE TABLE userAddress (
	  id INT(11) NOT NULL AUTO_INCREMENT
	, id_user INT(11) NOT NULL
	, id_street INT(11) NOT NULL
	, building VARCHAR(100) NOT NULL
	, PRIMARY KEY (id)
	, KEY id_user(id_user)
	, KEY id_street(id_street)
	, CONSTRAINT fk_user FOREIGN KEY (id_user) REFERENCES user(id)
	, CONSTRAINT fk_street FOREIGN KEY (id_street) REFERENCES street(id)
)
```


