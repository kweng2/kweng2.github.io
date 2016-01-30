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

So, alternatively, a Postgres container can be set up like this:

```
FROM postgres

USER postgres

RUN    /etc/init.d/postgresql start &&\
    psql --command "CREATE USER docker WITH SUPERUSER PASSWORD 'docker';" &&\
    createdb -O docker docker

EXPOSE 5432

CMD ["postgres"]
```



