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
```

``` mongoshell
flights> db.flightData.find({distance: {$gt: 934}}, {departureAirport: 1, arrivalAirport: 1, _id: 0})
[
  { departureAirport: 'MUC', arrivalAirport: 'SFO' },
  { departureAirport: 'LHR', arrivalAirport: 'TXL' }
]
flights>
```

``` mongoshell
flights> db.flightData.find({intercontinental: true})
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
flights> db.flightData.find({intercontinental: true}).count()
1
```

``` mongoshell
flights> db.flightData.updateOne({_id: ObjectId("64f0aa27c01f6d986549334e")}, {$set: {delayed: true}})
{
  acknowledged: true,
  insertedId: null,
  matchedCount: 1,
  modifiedCount: 1,
  upsertedCount: 0
}
```

|  |  | example |
| :---: | --- | ---: |
|update|overwritten other objects|db.flightData.update({_id: ObjectId("64f0aa27c01f6d986549334e")}, {delayed: true})|
|updateOne||db.flightData.updateOne({_id: ObjectId("64f0aa27c01f6d986549334e")}, {$set: {delayed: true}})|
|updateMany|||

<details>
<summary>insertMany Data</summary>

``` mongoshell
[
    {
        "name": "Max Schwarzmueller",
        "age": 29
    },
    {
        "name": "Manu Lorenz",
        "age": 30
    },
    {
        "name": "Chris Hayton",
        "age": 35
    },
    {
        "name": "Sandeep Kumar",
        "age": 28
    },
    {
        "name": "Maria Jones",
        "age": 30
    },
    {
        "name": "Alexandra Maier",
        "age": 27
    },
    {
        "name": "Dr. Phil Evans",
        "age": 47
    },
    {
        "name": "Sandra Brugge",
        "age": 33
    },
    {
        "name": "Elisabeth Mayr",
        "age": 29
    },
    {
        "name": "Frank Cube",
        "age": 41
    },
    {
        "name": "Karandeep Alun",
        "age": 48
    },
    {
        "name": "Michaela Drayer",
        "age": 39
    },
    {
        "name": "Bernd Hoftstadt",
        "age": 22
    },
    {
        "name": "Scott Tolib",
        "age": 44
    },
    {
        "name": "Freddy Melver",
        "age": 41
    },
    {
        "name": "Alexis Bohed",
        "age": 35
    },
    {
        "name": "Melanie Palace",
        "age": 27
    },
    {
        "name": "Armin Glutch",
        "age": 35
    },
    {
        "name": "Klaus Arber",
        "age": 53
    },
    {
        "name": "Albert Twostone",
        "age": 68
    },
    {
        "name": "Gordon Black",
        "age": 38
    }
]
```

```
db.passengers.insertMany([ { "name": "Max Schwarzmueller", "age": 29 }, { "name": "Manu Lorenz", "age": 30 }, { "name": "Chris Hayton", "age": 35 }, { "name": "Sandeep Kumar", "age": 28 }, { "name": "Maria Jones", "age": 30 }, { "name": "Alexandra Maier", "age": 27 }, { "name": "Dr. Phil Evans", "age": 47 }, { "name": "Sandra Brugge", "age": 33 }, { "name": "Elisabeth Mayr", "age": 29 }, { "name": "Frank Cube", "age": 41 }, { "name": "Karandeep Alun", "age": 48 }, { "name": "Michaela Drayer", "age": 39 }, { "name": "Bernd Hoftstadt", "age": 22 }, { "name": "Scott Tolib", "age": 44 }, { "name": "Freddy Melver", "age": 41 }, { "name": "Alexis Bohed", "age": 35 }, { "name": "Melanie Palace", "age": 27 }, { "name": "Armin Glutch", "age": 35 }, { "name": "Klaus Arber", "age": 53 }, { "name": "Albert Twostone", "age": 68 }, { "name": "Gordon Black", "age": 38 }] )
```

</details>

<details>
<summary>forEach</summary>

``` moogoshell
flights> db.passengers.find().forEach((passengerData) => {printjson(passengerData)})
{
  _id: ObjectId("64f2b9d7c01f6d9865493350"),
  name: 'Elisabeth Mayr',
  age: 29
}
{
  _id: ObjectId("64f2b9d7c01f6d9865493351"),
  name: 'Frank Cube',
  age: 41
}
{
  _id: ObjectId("64f2b9d7c01f6d9865493352"),
  name: 'Karandeep Alun',
  age: 48
}
{
  _id: ObjectId("64f2b9d7c01f6d9865493353"),
  name: 'Michaela Drayer',
  age: 39
}
{
  _id: ObjectId("64f2b9d7c01f6d9865493354"),
  name: 'Bernd Hoftstadt',
  age: 22
}
{
  _id: ObjectId("64f2b9d7c01f6d9865493355"),
  name: 'Scott Tolib',
  age: 44
}
{
  _id: ObjectId("64f2b9d7c01f6d9865493356"),
  name: 'Freddy Melver',
  age: 41
}
{
  _id: ObjectId("64f2b9d7c01f6d9865493357"),
  name: 'Alexis Bohed',
  age: 35
}
{
  _id: ObjectId("64f2b9d7c01f6d9865493358"),
  name: 'Melanie Palace',
  age: 27
}
{
  _id: ObjectId("64f2b9d7c01f6d9865493359"),
  name: 'Armin Glutch',
  age: 35
}
{
  _id: ObjectId("64f2b9d7c01f6d986549335a"),
  name: 'Klaus Arber',
  age: 53
}
{
  _id: ObjectId("64f2b9d7c01f6d986549335b"),
  name: 'Albert Twostone',
  age: 68
}
{
  _id: ObjectId("64f2b9d7c01f6d986549335c"),
  name: 'Gordon Black',
  age: 38
}
{
  _id: ObjectId("64f2bc29c01f6d986549335d"),
  name: 'Max Schwarzmueller'
}
{
  _id: ObjectId("64f2bc29c01f6d986549335e"),
  name: 'Manu Lorenz',
  age: 30
}
{
  _id: ObjectId("64f2bc29c01f6d986549335f"),
  name: 'Chris Hayton',
  age: 35
}
{
  _id: ObjectId("64f2bc29c01f6d9865493360"),
  name: 'Sandeep Kumar',
  age: 28
}
{
  _id: ObjectId("64f2bc29c01f6d9865493361"),
  name: 'Maria Jones',
  age: 30
}
{
  _id: ObjectId("64f2bc29c01f6d9865493362"),
  name: 'Alexandra Maier',
  age: 27
}
{
  _id: ObjectId("64f2bc29c01f6d9865493363"),
  name: 'Dr. Phil Evans',
  age: 47
}
{
  _id: ObjectId("64f2bc29c01f6d9865493364"),
  name: 'Sandra Brugge',
  age: 33
}
{
  _id: ObjectId("64f2bc29c01f6d9865493365"),
  name: 'Elisabeth Mayr',
  age: 29
}
{
  _id: ObjectId("64f2bc29c01f6d9865493366"),
  name: 'Frank Cube',
  age: 41
}
{
  _id: ObjectId("64f2bc29c01f6d9865493367"),
  name: 'Karandeep Alun',
  age: 48
}
{
  _id: ObjectId("64f2bc29c01f6d9865493368"),
  name: 'Michaela Drayer',
  age: 39
}
{
  _id: ObjectId("64f2bc29c01f6d9865493369"),
  name: 'Bernd Hoftstadt',
  age: 22
}
{
  _id: ObjectId("64f2bc29c01f6d986549336a"),
  name: 'Scott Tolib',
  age: 44
}
{
  _id: ObjectId("64f2bc29c01f6d986549336b"),
  name: 'Freddy Melver',
  age: 41
}
{
  _id: ObjectId("64f2bc29c01f6d986549336c"),
  name: 'Alexis Bohed',
  age: 35
}
{
  _id: ObjectId("64f2bc29c01f6d986549336d"),
  name: 'Melanie Palace',
  age: 27
}
{
  _id: ObjectId("64f2bc29c01f6d986549336e"),
  name: 'Armin Glutch',
  age: 35
}
{
  _id: ObjectId("64f2bc29c01f6d986549336f"),
  name: 'Klaus Arber',
  age: 53
}
{
  _id: ObjectId("64f2bc29c01f6d9865493370"),
  name: 'Albert Twostone',
  age: 68
}
{
  _id: ObjectId("64f2bc29c01f6d9865493371"),
  name: 'Gordon Black',
  age: 38
}
```

</details>

```
flights> db.flightData.updateMany({}, {$set: {status: {description: "on-time", lastUpdated: "1 hour ago"}}})
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
    _id: ObjectId("64f0aa27c01f6d986549334e"),
    departureAirport: 'MUC',
    arrivalAirport: 'SFO',
    aircraft: 'Airbus A380',
    distance: 12000,
    intercontinental: true,
    delayed: true,
    status: { description: 'on-time', lastUpdated: '1 hour ago' }
  },
  {
    _id: ObjectId("64f0aa27c01f6d986549334f"),
    departureAirport: 'LHR',
    arrivalAirport: 'TXL',
    aircraft: 'Airbus A320',
    distance: 950,
    intercontinental: false,
    status: { description: 'on-time', lastUpdated: '1 hour ago' }
  }
]
flights> db.passengers.updateOne({name: "Albert Twostone"}, {$set: {hobbies: ["sports", "cookking"]}})
{
  acknowledged: true,
  insertedId: null,
  matchedCount: 1,
  modifiedCount: 1,
  upsertedCount: 0
}
flights> db.passengers.find({name: "Albert Twostone"})
[
  {
    _id: ObjectId("64f2c018c01f6d9865493385"),
    name: 'Albert Twostone',
    age: 68,
    hobbies: [ 'sports', 'cookking' ]
  }
]
flights> db.passengers.find({name: "Albert Twostone"}).pretty()
[
  {
    _id: ObjectId("64f2c018c01f6d9865493385"),
    name: 'Albert Twostone',
    age: 68,
    hobbies: [ 'sports', 'cookking' ]
  }
]
flights> db.passengers.findOne({name: "Albert Twostone"}).hobbies
[ 'sports', 'cookking' ]
flights> db.passengers.find({hobbies: "sports"})
[
  {
    _id: ObjectId("64f2c018c01f6d9865493385"),
    name: 'Albert Twostone',
    age: 68,
    hobbies: [ 'sports', 'cookking' ]
  }
]
```

``` mongoshell
flights> db.flightData.find({"status.description": "on-time"})
[
  {
    _id: ObjectId("64f0aa27c01f6d986549334e"),
    departureAirport: 'MUC',
    arrivalAirport: 'SFO',
    aircraft: 'Airbus A380',
    distance: 12000,
    intercontinental: true,
    delayed: true,
    status: { description: 'on-time', lastUpdated: '1 hour ago' }
  },
  {
    _id: ObjectId("64f0aa27c01f6d986549334f"),
    departureAirport: 'LHR',
    arrivalAirport: 'TXL',
    aircraft: 'Airbus A320',
    distance: 950,
    intercontinental: false,
    status: { description: 'on-time', lastUpdated: '1 hour ago' }
  }
]
```
