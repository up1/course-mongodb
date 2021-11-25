# Authentication with MongoDB

## Basic authentication

Start MongoDB with Authentication
```
$mongod -f single.conf --auth
```

Create a new user with Local Exception (on localhost only)
```
$mongo
$use admin
$db.createUser({ user: "demo01", pwd: "demo01", roles: [{role: "userAdminAnyDatabase", db: "admin"}]})
```

Try to authenticate with a new user/pass
```
$db.auth('demo01','demo01')
```

Find all user in system
```
$db.system.users.find()
```

Exit from Mongo shell and try to connect again
```
$mongod --help
$mongo -u demo01 -p demo01
```



## Reference Websites
* [Connection String URI Format](https://docs.mongodb.com/manual/reference/connection-string/)

