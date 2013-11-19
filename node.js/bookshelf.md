# guidelines for using bookshelf with nodejs
This guide shows some simple examples to use bookshelf. For a complete reference see: http://bookshelfjs.org/.

## Getting Started
Bookshelf is an ORM for node.js. It supports transactions, eager/nested-eager relation loading.

### Initialize Bookshelf

            Bookshelf = Bookshelf.initialize({
                client: 'mysql',
                connection: {
                    host     : project.config.model.hosts[0].host
                    , user     : project.config.model.hosts[0].user
                    , password : project.config.model.hosts[0].password
                    , database : project.config.model.database
                    , port     : project.config.model.hosts[0].port
                }
            });

### Defining Models

	     var User = Bookshelf.Model.extend({
                  tableName: 'user'
            });


### Defining Relations

	

            var User = Bookshelf.Model.extend({

                  tableName: 'user'
                , userMailLogin: function() {
                    return this.hasOne(UserLoginEmail, "id_user");
                }

            });

            var District = Bookshelf.Model.extend({
                tableName: "district"
                , municipality: function() {
                    return this.hasMany(Municipality, "id_district");
                  }

            });

### Eager Loading
With Bookshelf it is possible to eager load:

            new County({id: 1}).fetch({
                withRelated: ["district.municipality"]
            }).exec(function(err, model) {
                log(JSON.stringify(model));
            });

This will produce the following output:
{
	"id":1,
	"id_country":1,
	"code":"be",
	"district":[{
	"id":1,
	"id_county":1,
	"municipality":[{
	"id":1,
	"id_district":1,
	"zip":"3011"}]}]
}

## Use standart node.js callbacks
To use standart node.js callbacks you need to call Bookshelf.plugin(require('bookshelf/plugins/exec')) after initializing Bookshelf. All Models defined after this command will have the then() and the exec() method. Where the exec() method takes the standart (err, result) callback interface.






