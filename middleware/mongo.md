# Mongo

- show dbs 

```
db.createUser (
	{
		user: "springbucks",
		pwd: "springbucks",
		roles : [
			{
				role:"readWrite",
				db:"springbucks"
			}
		]
	}
);

```
