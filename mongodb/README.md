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

``` PowerShell
> mongoimport tv-shows.json -d movies -c movieData --jsonArray --drop
2023-09-03T15:01:35.681+0900    connected to: mongodb://localhost/
2023-09-03T15:01:35.744+0900    dropping: movies.movieData
2023-09-03T15:01:35.869+0900    240 document(s) imported successfully. 0 document(s) failed to import.
```

``` mongoshell
test> use movieData
switched to db movieData
movieData> db.dropDatabase()
{ ok: 1, dropped: 'movieData' }
movieData> show dbs
```

- top 10 shows with the highest rating

```
db.movieData.find({}, { "name": 1, "url": 1, "rating.average": 1, "_id": 0 }).sort({ "rating.average": -1 }).limit(10)
```

- top 10 shows with the latest premiered date

``` mongoshell
movies> db.movieData.find({}, { "name": 1, "url": 1, "premiered": 1, "_id": 0 }).sort({ "premiered": -1 }).limit(10)
[
  {
    url: 'http://www.tvmaze.com/shows/159/state-of-affairs',
    name: 'State of Affairs',
    premiered: '2014-11-17'
  },
  {
    url: 'http://www.tvmaze.com/shows/136/the-mccarthys',
    name: 'The McCarthys',
    premiered: '2014-10-30'
  },
  {
    url: 'http://www.tvmaze.com/shows/241/benched',
    name: 'Benched',
    premiered: '2014-10-28'
  },
  {
    url: 'http://www.tvmaze.com/shows/15/constantine',
    name: 'Constantine',
    premiered: '2014-10-24'
  },
  {
    url: 'http://www.tvmaze.com/shows/242/the-great-fire',
    name: 'The Great Fire',
    premiered: '2014-10-16'
  },
  {
    url: 'http://www.tvmaze.com/shows/129/marry-me',
    name: 'Marry Me',
    premiered: '2014-10-14'
  },
  {
    url: 'http://www.tvmaze.com/shows/128/jane-the-virgin',
    name: 'Jane the Virgin',
    premiered: '2014-10-13'
  },
  {
    url: 'http://www.tvmaze.com/shows/127/the-affair',
    name: 'The Affair',
    premiered: '2014-10-12'
  },
  {
    url: 'http://www.tvmaze.com/shows/137/cristela',
    name: 'Cristela',
    premiered: '2014-10-10'
  },
  {
    url: 'http://www.tvmaze.com/shows/214/kingdom',
    name: 'Kingdom',
    premiered: '2014-10-08'
  }
]
movies>
```

``` mongoshell
movies> db.movieData.find({$or: [{"rating.average": {$lt: 5}}, {"rating.average": {$gt: 9.3}}]}).limit(10)
[
  {
    _id: ObjectId("64f4213f3e2ad75079a2a721"),
    id: 71,
    url: 'http://www.tvmaze.com/shows/71/dancing-with-the-stars',
    name: 'Dancing with the Stars',
    type: 'Reality',
    language: 'English',
    genres: [ 'Music' ],
    status: 'Running',
    runtime: 120,
    premiered: '2005-06-01',
    officialSite: 'http://abc.go.com/shows/dancing-with-the-stars',
    schedule: { time: '20:00', days: [ 'Monday' ] },
    rating: { average: 4.7 },
    weight: 84,
    network: {
      id: 3,
      name: 'ABC',
      country: {
        name: 'United States',
        code: 'US',
        timezone: 'America/New_York'
      }
    },
    webChannel: null,
    externals: { tvrage: 3220, thetvdb: 79590, imdb: 'tt0463398' },
    image: {
      medium: 'http://static.tvmaze.com/uploads/images/medium_portrait/0/501.jpg',
      original: 'http://static.tvmaze.com/uploads/images/original_untouched/0/501.jpg'
    },
    summary: '<p><b>Dancing with the Stars</b> is an american dance competition show and especially the american version of the british show <i>Strictly Come Dancing</i>.</p>',
    updated: 1532455112,
    _links: {
      self: { href: 'http://api.tvmaze.com/shows/71' },
      previousepisode: { href: 'http://api.tvmaze.com/episodes/1446662' },
      nextepisode: { href: 'http://api.tvmaze.com/episodes/1501076' }
    }
  },
  {
    _id: ObjectId("64f4213f3e2ad75079a2a728"),
    id: 82,
    url: 'http://www.tvmaze.com/shows/82/game-of-thrones',
    name: 'Game of Thrones',
    type: 'Scripted',
    language: 'English',
    genres: [ 'Drama', 'Adventure', 'Fantasy' ],
    status: 'Running',
    runtime: 60,
    premiered: '2011-04-17',
    officialSite: 'http://www.hbo.com/game-of-thrones',
    schedule: { time: '21:00', days: [ 'Sunday' ] },
    rating: { average: 9.4 },
    weight: 99,
    network: {
      id: 8,
      name: 'HBO',
      country: {
        name: 'United States',
        code: 'US',
        timezone: 'America/New_York'
      }
    },
    webChannel: {
      id: 22,
      name: 'HBO Go',
      country: {
        name: 'United States',
        code: 'US',
        timezone: 'America/New_York'
      }
    },
    externals: { tvrage: 24493, thetvdb: 121361, imdb: 'tt0944947' },
    image: {
      medium: 'http://static.tvmaze.com/uploads/images/medium_portrait/143/359013.jpg',
      original: 'http://static.tvmaze.com/uploads/images/original_untouched/143/359013.jpg'
    },
    summary: '<p>Based on the bestselling book series <i>A Song of Ice and Fire</i> by George R.R. Martin, this sprawling new HBO drama is set in a world where summers span decades and winters can last a lifetime. From the scheming south and the savage eastern lands, to the frozen north and ancient Wall that protects the realm from the mysterious darkness beyond, the powerful families of the Seven Kingdoms are locked in a battle for the Iron Throne. This is a story of duplicity and treachery, nobility and honor, conquest and triumph. In the <b>Game of Thrones</b>, you either win or you die.</p>',
    updated: 1532947493,
    _links: {
      self: { href: 'http://api.tvmaze.com/shows/82' },
      previousepisode: { href: 'http://api.tvmaze.com/episodes/1221415' }
    }
  },
  {
    _id: ObjectId("64f4213f3e2ad75079a2a790"),
    id: 193,
    url: 'http://www.tvmaze.com/shows/193/dads',
    name: 'Dads',
    type: 'Scripted',
    language: 'English',
    genres: [ 'Comedy' ],
    status: 'Ended',
    runtime: 30,
    premiered: '2013-09-17',
    officialSite: null,
    schedule: { time: '20:00', days: [ 'Tuesday' ] },
    rating: { average: 4.4 },
    weight: 55,
    network: {
      id: 4,
      name: 'FOX',
      country: {
        name: 'United States',
        code: 'US',
        timezone: 'America/New_York'
      }
    },
    webChannel: null,
    externals: { tvrage: 34573, thetvdb: 269589, imdb: 'tt2647548' },
    image: {
      medium: 'http://static.tvmaze.com/uploads/images/medium_portrait/1/2831.jpg',
      original: 'http://static.tvmaze.com/uploads/images/original_untouched/1/2831.jpg'
    },
    summary: "<p>Honor thy father. Way easier said than done. Especially when your dad's broke, living in your house and ruining your life. <b>Dads</b> explores the often treacherous terrain of the father-son landscape. This series follows two successful guys - and childhood best friends - now in their mid-30s whose relatively stable lives get turned upside down when their pain-in-the-neck patriarchs move in.</p>",
    updated: 1533638929,
    _links: {
      self: { href: 'http://api.tvmaze.com/shows/193' },
      previousepisode: { href: 'http://api.tvmaze.com/episodes/13177' }
    }
  },
  {
    _id: ObjectId("64f4213f3e2ad75079a2a7a8"),
    id: 216,
    url: 'http://www.tvmaze.com/shows/216/rick-and-morty',
    name: 'Rick and Morty',
    type: 'Animation',
    language: 'English',
    genres: [ 'Comedy', 'Adventure', 'Science-Fiction' ],
    status: 'Running',
    runtime: 30,
    premiered: '2013-12-02',
    officialSite: 'http://www.adultswim.com/videos/rick-and-morty',
    schedule: { time: '23:30', days: [ 'Sunday' ] },
    rating: { average: 9.4 },
    weight: 92,
    network: {
      id: 10,
      name: 'Adult Swim',
      country: {
        name: 'United States',
        code: 'US',
        timezone: 'America/New_York'
      }
    },
    webChannel: null,
    externals: { tvrage: 33381, thetvdb: 275274, imdb: 'tt2861424' },
    image: {
      medium: 'http://static.tvmaze.com/uploads/images/medium_portrait/1/3603.jpg',
      original: 'http://static.tvmaze.com/uploads/images/original_untouched/1/3603.jpg'
    },
    summary: '<p>Rick is a mentally gifted, but sociopathic and alcoholic scientist and a grandfather to Morty; an awkward, impressionable, and somewhat spineless teenage boy. Rick moves into the family home of Morty, where he immediately becomes a bad influence.</p>',
    updated: 1533638739,
    _links: {
      self: { href: 'http://api.tvmaze.com/shows/216' },
      previousepisode: { href: 'http://api.tvmaze.com/episodes/1285113' }
    }
  }
]
movies>
```

``` mongoshell
db.movieData.find({$and: [{"rating.average": {$gt: 9.3}}, {genres: "Drama"}]}).limit(10)
```

``` mongoshell
db.movieData.find({"genres": "Anime"}, {"genres": 1, "name": 1, "rating.average": 1, "_id": 0}).sort({"rating.average": -1}).limit(10)
```

``` mongoshell
# same results
db.movieData.find({runtime: {$not: {$eq: 60}}}).count()
db.movieData.find({runtime: {$ne: 60}}).count()
70
```

``` mongoshell
db.passengers.find({age: {$exists: true, $gt: 60}})
```

``` mongoshell
db.passengers.find({age: {$exists: true, $ne: null}}).count()
```

``` mongoshell
db.passengers.find({age: {$type: "number"}}).count()
```

``` mongoshell
db.passengers.find({age: {$type: ["number", "string"]}}).count()
```

``` mongoshell
db.movieData.find({summary: {$regex: /musical/}})
```

``` mongoshell
db.movieData.find({summary: {$regex: /musical/}}, {name: 1, summary: 1, _id: 0})
```

``` mongoshell
db.sales.insertMany([{volume: 100, target: 120}, {volume: 89, target: 80}, {volume: 200, target: 177}])
```

``` mongoshell
financials> db.sales.find({$expr: {$gt: ["$volume", "$target"]}})
[
  { _id: ObjectId("64f9bd7bc67eb444b5ebad4f"), volume: 89, target: 80 },
  {
    _id: ObjectId("64f9bd7bc67eb444b5ebad50"),
    volume: 200,
    target: 177
  }
]
financials>
```

``` mongoshell
financials> db.sales.find({$expr: {$gt: [{$cond: {if: {$gte: ["$volume", 190]}, then: {$subtract: ["$volume", 10]}, else: "$volume" }}, "$target"]}})
[
  { _id: ObjectId("64f9bd7bc67eb444b5ebad4f"), volume: 89, target: 80 },
  {
    _id: ObjectId("64f9bd7bc67eb444b5ebad50"),
    volume: 200,
    target: 177
  }
]
financials>
```

``` mongoshell
```

``` mongoshell
```

``` mongoshell
```

``` mongoshell
```
