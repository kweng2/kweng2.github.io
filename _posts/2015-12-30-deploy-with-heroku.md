---
title: Deploy with Heroku
updated: 2015-12-30
---

Wrote an API containing student information, and deployed onto heroku:

To query all students, use:
[https://fierce-harbor-4835.herokuapp.com](https://fierce-harbor-4835.herokuapp.com)

### Accepts request at:
* /students/cohort/NUMBER
* /students/id/STRING

### Accepts these types of requests:
* GET to /students, /students/id, and /students/cohort
* POST to /students
* PUT to /students/id

### Useful Reminder
While deploying to Heroku, when adding an add-on, like MongoLab, make sure to set config variables in terminal:

```
$ heroku config:set MONGOLAB_URI=?????
```
Replace ```?????``` with config variables, accessible through Heroku
