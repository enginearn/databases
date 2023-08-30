# Install MongoDB

## Windows

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
