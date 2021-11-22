# Start a single instance of MongoDB
* Download and install [MongoDB Server Community](https://docs.mongodb.com/manual/administration/install-community/)
* [Installing the Database Tools](https://docs.mongodb.com/database-tools/installation/installation/)


```
$cd bin
$mkdir -p mongodb/db/node1
$touch single.conf
```

Confin in file `single.conf`
```
storage:
  dbPath: mongodb/db/node1
```

Start MongoDB Server with mongod
```
$mongod -f single.conf
```

Connect from mongo shell
```
$mongo
```


Let's start with MongoDB ...