# About

Abstract your databases, make datababies.  ueberDB turns every database into a simple key value store by providing a layer of abstraction between your software and your database.  ueberDB uses a smart cache and buffer algorithm to make databases faster. Reads are cached and writes are done in a bulk. The bulk writing reduces the overhead of a database transaction.  The syntax is simple and clean and getting started is easy.

# Database Support
* Couch
* Crate
* Dirty
* Elasticsearch
* Level
* Mongo
* MySQL (<= 5.7)
* Postgres (single connection and with connection pool)
* CockroachDB (use the postgres_db module)
* Redis
* RethinkDB
* SQLite

# Install

`npm install ueberDB`

# Example

```javascript
var ueberDB = require("ueberdb2");

//mysql
var db = new ueberDB.database("mysql", {"user":"root", host: "localhost", "password":"", database: "store"});
//sqlite in-memory
//var db = new ueberDB.database("sqlite");
//sqlite in file
//var db = new ueberDB.database("sqlite", {filename:"var/sqlite3.db"});
//sqlite in file with a write interval of a half second
//var db = new ueberDB.database("sqlite", {filename:"var/sqlite3.db"}, {writeInterval: 500});

//initialize the database
db.init(async function (err)
{
  if(err)
  {
    console.error(err);
    process.exit(1);
  }

  //set a object as a value
  //can be done without a callback, cause the value is immediately in the buffer
  await db.set("valueA", {a:1,b:2});

  //get the object
  db.get("valueA", function(err, value){
    console.log(value);

    db.close(function(){
      process.exit(0);
    });
  });
});
```

# How to add support for another database
Look at sqlite_db.js and mysql_db.js, your module have to provide the same functions. Call it DATABASENAME_db.js and reimplement the functions for your database. If you think it works, test it with `node benchmark.js DATABASENAME`. Benchmark.js is benchmark and test at the same time. It tries to set 100000 values. You can pipe stderr to a file and will create a csv with benchmark results.

# Limitations
Only mysql, dirty, mongodb, couch, rethink and crate currently support findKeys feature. The following do not yet support the function:

* leveldb
* redis (Only keys of the format \*:\*:\*)
* cassandra (Only keys of the format \*:\*:\*)
* elasticsearch (Only keys of the format \*:\*:\*)

For details on how it works please refer to the wiki: https://github.com/ether/ueberDB/wiki/findKeys-functionality

# MySQL /MariaDB Advice
You should create your database as utf8mb4_bin.
Also, please ensure `settings.json` (`dbSettings.charset`) is configured to use the same charset that your database uses, otherwise ueberDB will complain at startup.

# License
[Apache License v2](http://www.apache.org/licenses/LICENSE-2.0.html)
