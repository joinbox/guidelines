# SQL Schema Styleguide

This Styleguide defines how tables should be defined.



#### Naming

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

Table & column names must be camelCase except for names referencing another entity.


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