# guidelines for using bookshelf with nodejs

## Getting Started

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






