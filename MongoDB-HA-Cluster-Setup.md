# Setup MongoDB HA ReplicaSet


## In this tutorial, the three mongod instances are associated with the following hosts:

|Replica Set Member	| Hostname|
| ------------- | ------------- |
|Member 0	| mongodb0.deepshah.me|
|Member 1	| mongodb1.deepshah.me|
|Member 2	| mongodb2.deepshah.me|

## Installing MongoDB

1. Import the public key used by the package management system
```
wget -qO - https://www.mongodb.org/static/pgp/server-4.4.asc | sudo apt-key add -
```

2. Create a /etc/apt/sources.list.d/mongodb-org-4.4.list
```
echo "deb http://repo.mongodb.org/apt/debian buster/mongodb-org/4.4 main" | sudo tee /etc/apt/sources.list.d/mongodb-org-4.4.list
```

3. Reload local package database and install MongoDB latest version.
```
sudo apt-get update && sudo apt-get install -y mongodb-org
```

## Configuring the Replica Set

1. Start each member of the replica set with the appropriate options

```
replication:
   replSetName: "rs0"
net:
   bindIp: localhost,0.0.0.0
```
```
sudo systemctl start mongod
```
```
sudo systemctl status mongod
```
```
sudo systemctl enable mongod
```


2. Connect a mongo shell to one of the mongod instances.

```
mongo
```

3. Initiate the replica set.

```Note:- Run rs.initiate() on just one and only one mongod instance for the replica set.```

```
rs.initiate( {
   _id : "rs0",
   members: [
      { _id: 0, host: "mongodb0.deepshah.me:27017" },
      { _id: 1, host: "mongodb1.deepshah.me:27017" },
      { _id: 2, host: "mongodb2.deepshah.me:27017" }
   ]
})
```

4. View the replica set configuration.

Use rs.conf() to display the replica set configuration object:
```
rs.conf()
```
The replica set configuration object resembles the following:
```
{
   "_id" : "rs0",
   "version" : 1,
   "protocolVersion" : NumberLong(1),
   "members" : [
      {
         "_id" : 0,
         "host" : "mongodb0.example.net:27017",
         "arbiterOnly" : false,
         "buildIndexes" : true,
         "hidden" : false,
         "priority" : 1,
         "tags" : {

         },
         "slaveDelay" : NumberLong(0),
         "votes" : 1
      },
      {
         "_id" : 1,
         "host" : "mongodb1.example.net:27017",
         "arbiterOnly" : false,
         "buildIndexes" : true,
         "hidden" : false,
         "priority" : 1,
         "tags" : {

         },
         "slaveDelay" : NumberLong(0),
         "votes" : 1
      },
      {
         "_id" : 2,
         "host" : "mongodb2.example.net:27017",
         "arbiterOnly" : false,
         "buildIndexes" : true,
         "hidden" : false,
         "priority" : 1,
         "tags" : {

         },
         "slaveDelay" : NumberLong(0),
         "votes" : 1
      }

   ],
   "settings" : {
      "chainingAllowed" : true,
      "heartbeatIntervalMillis" : 2000,
      "heartbeatTimeoutSecs" : 10,
      "electionTimeoutMillis" : 10000,
      "catchUpTimeoutMillis" : -1,
      "getLastErrorModes" : {

      },
      "getLastErrorDefaults" : {
         "w" : 1,
         "wtimeout" : 0
      },
      "replicaSetId" : ObjectId("585ab9df685f726db2c6a840")
   }
}
```
5. Ensure that the replica set has a primary.

Use rs.status() to identify the primary in the replica set.
```
rs.status()
```
Setting Priority (from the PRIMARY)
```
cfg = rs.conf()
cfg.members[0].priority = 0.5
cfg.members[1].priority = 0.5
cfg.members[2].priority = 1
rs.reconfig(cfg)
```
Force a Member to be Primary by Setting its Priority High
1. Freeze the other secondary so that it doesnt become primary.
```
rs.freeze(120)
```
2. Make the current Primary step down
```
rs.stepDown(120)
```
