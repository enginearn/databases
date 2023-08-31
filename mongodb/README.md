# MongoDB

## Installing on Windows

1. Download the latest version of MongoDB from [here](https://www.mongodb.com/try/download/community).
2. Install MongoDB. `C:\Program Files\MongoDB`
3. Create a data folder `C:\Users\path\to\DBs\MongoDB\data`.
4. Create a log folder `C:\Users\path\to\DBs\MongoDB\log`.
5. Add `C:\Program Files\MongoDB\Server\7.0\bin` to the `PATH` environment variable.
6. Open Compass and connect to the MongoDB server.
7. Run `mongosh` to start the MongoDB shell.

    ```PowerShell
    > mongosh
   Current Mongosh Log ID: 64ef619c917c98b5740ebfb0
   Connecting to:          mongodb://127.0.0.1:27017/?directConnection=true&serverSelectionTimeoutMS=2000&appName=mongosh+1.10.6
   Using MongoDB:          7.0.0
   Using Mongosh:          1.10.6

   For mongosh info see: https://docs.mongodb.com/mongodb-shell/

   ------
      The server generated these startup warnings when booting
      2023-08-30T23:49:08.569+09:00: Access control is not enabled for the database. Read and write access to data and configuration is unrestricted
   ------

   test> Ctrl+C to exit
   (To exit, press Ctrl+C again or Ctrl+D or type .exit)
    ```

8. Run `use admin` to switch to the `admin` database.

## CRUD Basics

``` mongoshell
test> use shop
switched to db shop
shop> db.products.insertOne({name: "A Book", price: 12.99})
{
  acknowledged: true,
  insertedId: ObjectId("64f0901fc01f6d9865493349")
}
shop> db.products.insertOne({name: "A T-Shirt", price: 29.99, description: "This is a high quality T-Shirt!"})
{
  acknowledged: true,
  insertedId: ObjectId("64f09088c01f6d986549334a")
}
shop> db.products.insertOne({name: "A high spec computer", price: 1229.99, description: "This is a high quality computer", details: {cpu: "Ryzen 15", memory: 2048}})
{
  acknowledged: true,
  insertedId: ObjectId("64f0912ac01f6d986549334b")
}
shop>
```

``` mongoshell
shop> db.products.find().pretty()
[
  {
    _id: ObjectId("64f0901fc01f6d9865493349"),
    name: 'A Book',
    price: 12.99
  },
  {
    _id: ObjectId("64f09088c01f6d986549334a"),
    name: 'A T-Shirt',
    price: 29.99,
    description: 'This is a high quality T-Shirt!'
  },
  {
    _id: ObjectId("64f0912ac01f6d986549334b"),
    name: 'A high spec computer',
    price: 1229.99,
    description: 'This is a high quality computer',
    details: { cpu: 'Ryzen 15', memory: 2048 }
  }
]
shop>
```

``` mongoshell
shop> use flights
switched to db flights
flights> show dbs
admin   40.00 KiB
config  72.00 KiB
local   88.00 KiB
shop    72.00 KiB
lights> db.flightData.insertOne({"departureAirport": "MUC", "arrivalAirport": "SFO", "aircraft": "Airbus A380", "distance": 12000, "intercontinental": true})
{
  acknowledged: true,
  insertedId: ObjectId("64f0a0c1c01f6d986549334c")
}
flights> db.flightData.find()
[
  {
    _id: ObjectId("64f0a0c1c01f6d986549334c"),
    departureAirport: 'MUC',
    arrivalAirport: 'SFO',
    aircraft: 'Airbus A380',
    distance: 12000,
    intercontinental: true
  }
]
flights> db.flightData.find().pretty()
[
  {
    _id: ObjectId("64f0a0c1c01f6d986549334c"),
    departureAirport: 'MUC',
    arrivalAirport: 'SFO',
    aircraft: 'Airbus A380',
    distance: 12000,
    intercontinental: true
  }
]
flights> db.flightData.insertOne({departureAirport: "TXL", arrivalAirport: "LHR"})
{
  acknowledged: true,
  insertedId: ObjectId("64f0a225c01f6d986549334d")
}
flights> db.flightData.find().pretty()
[
  {
    _id: ObjectId("64f0a0c1c01f6d986549334c"),
    departureAirport: 'MUC',
    arrivalAirport: 'SFO',
    aircraft: 'Airbus A380',
    distance: 12000,
    intercontinental: true
  },
  {
    _id: ObjectId("64f0a225c01f6d986549334d"),
    departureAirport: 'TXL',
    arrivalAirport: 'LHR'
  }
]
flights> db.flightData.insertOne({departureAirport: "TXL", arrivalAirport: "LHR", _id: "txl-lhr-001"})
{ acknowledged: true, insertedId: 'txl-lhr-001' }
flights> db.flightData.find()
[
  {
    _id: ObjectId("64f0a0c1c01f6d986549334c"),
    departureAirport: 'MUC',
    arrivalAirport: 'SFO',
    aircraft: 'Airbus A380',
    distance: 12000,
    intercontinental: true
  },
  {
    _id: ObjectId("64f0a225c01f6d986549334d"),
    departureAirport: 'TXL',
    arrivalAirport: 'LHR'
  },
  {
    _id: 'txl-lhr-001',
    departureAirport: 'TXL',
    arrivalAirport: 'LHR'
  }
]
flights> db.flightData.insertOne({departureAirport: "TXL", arrivalAirport: "LHR", _id: "txl-lhr-001"})
MongoServerError: E11000 duplicate key error collection: flights.flightData index: _id_ dup key: { _id: "txl-lhr-001" }
flights>
```

``` mongoshell
flights> db.flightData.deleteOne({departureAirport: "TXL"})
{ acknowledged: true, deletedCount: 1 }
flights> db.flightData.find()
[
  {
    _id: ObjectId("64f0a0c1c01f6d986549334c"),
    departureAirport: 'MUC',
    arrivalAirport: 'SFO',
    aircraft: 'Airbus A380',
    distance: 12000,
    intercontinental: true
  },
  {
    _id: 'txl-lhr-001',
    departureAirport: 'TXL',
    arrivalAirport: 'LHR'
  }
]
flights> db.flightData.updateOne({distance: 12000}, {$set: {marker: "delete"}})
{
  acknowledged: true,
  insertedId: null,
  matchedCount: 1,
  modifiedCount: 1,
  upsertedCount: 0
}
flights>
```

``` mongoshell
flights> db.flightData.updateMany({}, {$set: {marker: "toDelete"}})
{
  acknowledged: true,
  insertedId: null,
  matchedCount: 2,
  modifiedCount: 2,
  upsertedCount: 0
}
flights> db.flightData.find()
[
  {
    _id: ObjectId("64f0a0c1c01f6d986549334c"),
    departureAirport: 'MUC',
    arrivalAirport: 'SFO',
    aircraft: 'Airbus A380',
    distance: 12000,
    intercontinental: true,
    marker: 'toDelete'
  },
  {
    _id: 'txl-lhr-001',
    departureAirport: 'TXL',
    arrivalAirport: 'LHR',
    marker: 'toDelete'
  }
]
flights> db.flightData.deleteMany({marker: "toDelete"})
{ acknowledged: true, deletedCount: 2 }
flights> db.flightData.find()

flights>
```

``` mongoshell
flights> db.flightData.insertMany([{"departureAirport": "MUC", "arrivalAirport": "SFO", "aircraft": "Airbus A380", "distance": 12000, "intercontinental": true}, {"departureAirport": "LHR","arrivalAirport": "TXL","aircraft": "Airbus A320","distance": 950,"intercontinental": false}])
{
  acknowledged: true,
  insertedIds: {
    '0': ObjectId("64f0aa27c01f6d986549334e"),
    '1': ObjectId("64f0aa27c01f6d986549334f")
  }
}
flights> db.flightData.find()
[
  {
    _id: ObjectId("64f0aa27c01f6d986549334e"),
    departureAirport: 'MUC',
    arrivalAirport: 'SFO',
    aircraft: 'Airbus A380',
    distance: 12000,
    intercontinental: true
  },
  {
    _id: ObjectId("64f0aa27c01f6d986549334f"),
    departureAirport: 'LHR',
    arrivalAirport: 'TXL',
    aircraft: 'Airbus A320',
    distance: 950,
    intercontinental: false
  }
]
flights> db.flightData.find({distance: {$gt: 10000})
Uncaught:
SyntaxError: Unexpected token, expected "," (1:42)

> 1 | db.flightData.find({distance: {$gt: 10000})
    |                                           ^
  2 |

flights> db.flightData.find({distance: {$gt: 10000}})
[
  {
    _id: ObjectId("64f0aa27c01f6d986549334e"),
    departureAirport: 'MUC',
    arrivalAirport: 'SFO',
    aircraft: 'Airbus A380',
    distance: 12000,
    intercontinental: true
  }
]
flights> db.flightData.find({distance: {$gt: 934}})
[
  {
    _id: ObjectId("64f0aa27c01f6d986549334e"),
    departureAirport: 'MUC',
    arrivalAirport: 'SFO',
    aircraft: 'Airbus A380',
    distance: 12000,
    intercontinental: true
  },
  {
    _id: ObjectId("64f0aa27c01f6d986549334f"),
    departureAirport: 'LHR',
    arrivalAirport: 'TXL',
    aircraft: 'Airbus A320',
    distance: 950,
    intercontinental: false
  }
]
flights>
```

``` mongoshell
flights> db.flightData.findOne({distance: {$gt: 934}})
{
  _id: ObjectId("64f0aa27c01f6d986549334e"),
  departureAirport: 'MUC',
  arrivalAirport: 'SFO',
  aircraft: 'Airbus A380',
  distance: 12000,
  intercontinental: true
}
flights>
