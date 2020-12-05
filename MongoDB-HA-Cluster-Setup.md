# Setup MongoDB HA ReplicaSet


## In this tutorial, the three mongod instances are associated with the following hosts:

|Replica Set Member	| Hostname|
| ------------- | ------------- |
|Member 0	| mongodb0.deepshah.me|
|Member 1	| mongodb1.deepshah.me|
|Member 2	| mongodb2.deepshah.me|

1. Start each member of the replica set with the appropriate options

```
replication:
   replSetName: "rs0"
net:
   bindIp: localhost,0.0.0.0
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
