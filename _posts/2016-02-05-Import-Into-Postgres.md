---
title: Importing CSV Into Postgres
updated: 2016-02-05
---

### CSV to Postgres

Often times it is exceedingly helpful to be able to seed a database with a ton of data. Conveniently, Postgres comes with a ```copy``` command that allows importing data into a table from a file, such as a .csv.

Inside the ```psql``` command line, enter the following:
```
COPY SOME_TABLE from 'FILE_PATH.csv' WITH DELIMITER ',' CSV HEADER;
```

### Pre-reqs:

1. Access to ```psql``` interactive terminal
1. A table with ```SOME_TABLE``` already exists
1. A csv file exists at some ```FILE_PATH.csv```
1. The csv file headers matches the table column names

##### Postgres table example:
```CREATE TABLE SOME_TABLE (ID integer, NAME varchar);```

##### CSV file example:

|id|name|
|---|---|
|0|'Kevin'|
|1|'Alan'|
|2|'Stephen'|

The ```COPY``` command copies the content of ```FILE_PATH.csv``` into ```SOME_TABLE```, with commas as delimiters, using a csv file with headers.

Checkout Postgres [docs](http://www.postgresql.org/docs/9.3/static/sql-copy.html) for detailed explainations of more advanced options.

This method assumes access to the psql interactive terminal.

### Importing In Node

Checkout the [pg-copy-streams](https://github.com/brianc/node-pg-copy-streams) node module to import from a file while inside node.

This method allows importing without direct access to the Postgres interactive terminal.

Here's an example use that copies from a file into the database:

```javascript
var fs = require('fs');
var pg = require('pg');
var copyFrom = require('pg-copy-streams').from;

pg.connect(CONNECT_STR, function(err, client, done) {
  var stream = client.query(copyFrom('COPY my_table FROM STDIN'));
  var fileStream = fs.createReadStream('some_file.tsv')
  fileStream.on('error', done);
  fileStream.pipe(stream).on('finish', done).on('error', done);
});
```

Happy hacking!
