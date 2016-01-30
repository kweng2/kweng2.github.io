---
title: Postgres Within Docker
updated: 2016-01-28
---

### Disclaimer!
Setting up Postgres requires a user, password, and a new database, which all need to be created in a Dockerfile. Otherwise, any server trying to connect to the database, despite correct port pointing, will not work.

### How to Set-up Postgres within Docker

Here is the Dockerfile that will create a Postgres database and set it up with user 'docker', password 'docker', and database 'docker'.

This code block comes from [docs.docker](https://docs.docker.com/engine/examples/postgresql_service/)

##### Create this docker container from an existing image, ubuntu:
```
FROM ubuntu
```

##### Install Postgres
```
RUN apt-key adv --keyserver hkp://p80.pool.sks-keyservers.net:80 --recv-keys B97B0AFCAA1A47F044F244A07FCC7D46ACCC4CF8

RUN echo "deb http://apt.postgresql.org/pub/repos/apt/ precise-pgdg main" > /etc/apt/sources.list.d/pgdg.list

RUN apt-get update && apt-get install -y python-software-properties software-properties-common postgresql-9.3 postgresql-client-9.3 postgresql-contrib-9.3
```

##### Log into Postgres as the default user, 'postgres':
```
USER postgres
```

##### Create a user named 'docker', with the password 'docker', then create a database named 'docker':
```
RUN    /etc/init.d/postgresql start &&\
    psql --command "CREATE USER docker WITH SUPERUSER PASSWORD 'docker';" &&\
    createdb -O docker docker
```

### Difficulties with Postgres Set-up
One of the most painful challenges to run into is setting up Postgres, because crushing that bug isn't even satisfying! So here is an easy to miss step that can be a real headache to debug, step 4 outlined above.

### Easier Solution, Less Options
So, alternatively, a Postgres docker container is already set up from the postgres image, available on [Docker Hub](https://hub.docker.com/_/postgres/)

Include it in the docker-compose.yml file:

```
postgres:
  image: postgres
```

So when ```docker-compose up``` is run, port 5432 is then exposed.

This image has configured a default user ```postgres``` with the password ```mysecretpassword```, and a default database '```postgres```'.

Due to these pre-existing configuration, connecting an ORM to this Postgres database is as simple as specifying the connection parameters as such:

```javascript
connection: {
  host: 'postgres',
  user: 'postgres',
  password: 'mysecretpassword',
  database: 'postgres',
  charset: 'utf8'
}
```

