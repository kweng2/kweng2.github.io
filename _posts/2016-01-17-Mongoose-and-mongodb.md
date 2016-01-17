---
title: Updating Entries in Mongoose/MongoDB
updated: 2016-01-17
---

### findOneAndUpdate

Updating an entry in MongoDB via Mongoose ORM can be conveniently achieved using the ```findOneAndUpdate``` function. Here's an example with object stores that take this form:

```javascript
var person = new mongoose.Schema({
  name: String,
  age: Number,
  education: Array
});

mongoose.model('Person', person);
```

To update that a particular person's age, we can simply write:

```javascript
Person.findOneAndUpdate({'name': 'Kevin'}, {age: 26});
```

In the above example, the update was rather straightforward; however if we were to update a particular entry in an array, the operation becomes more involved.

### Update an embedded array

For example, the education field, containing an array of objects describing the schools this person has attended may need to be updated. And we do not wish to update the entire array, but only an entry of it. How would we achieve that?

> *Disclaimer*
In many cases, it may make sense to use a SQL database since a larger dataset would have people and educational institutions stored as relational entries. However for the purpose of this post, exploring how to update a particular entry of the array, let's assume the database is formed this way.

Suppose this person went to GenericName High School, graduating in 2008, then to Famous University, graduating in 2012, followed by Some Research Institute, graduating in 2016. The eduction field of this person would then look like:

```javascript
[{
  _id: 76sd68f7g7
  school: 'GenericName High School',
  graduatingYear: 2008
}, {
  _id: 9a7df87s96
  school: 'Famous University',
  graduatingYear: 2012
}, {
  _id: 987sdf987s
  school: 'Some Research Institution,
  graduatingYear: 2016
}]
```
We can gain access to to the object via ```findOne``` then operate on the array as needed, then perform a ```.save()``` operation. So in case we want to highschool entry entirely, the code would take the following form:

```javascript
Person.findOne({name: 'Kevin'}, function (err, foundPerson) {
  for (var i = 0; i < foundPerson.education.length; i++) {
    if (foundPerson.education[i]._id === '76sd68f7g7') {
      foundPerson.education.splice(i, 1);
      foundPerson.education.splice(i, 0, {
        _id: 7a5fjh4871,
        school: 'Another High School',
        graduatingYear: 2008
      });
      foundPerson.save();
      return;
    }
  }
  // in case that person was not found
  return callback(Error('No such school with this _id'));
});
```
*Note!*
Why did we use splice operation twice instead of ```foundPerson.education[i] = {}``` directly?

Mongoose does not recognize that as a change on the foundPerson, and therefore the ```.save()``` operation would not actually save the updated person.

Using ```splice``` here forces Mongoose to recognize the change therefore the save function behaves correctly as a result.

### Built-in MongoDB Update methods

You might be wondering doesn't MongoDB support embedded field updates for arrays? The answer is yes. Checkout these pages for excellent explanations:

* [Updating Embedded Field in MongoDB](https://docs.mongodb.org/manual/tutorial/modify-documents/#update-an-embedded-field)
* [Dot Notation in MongoDB](https://docs.mongodb.org/manual/core/document/#dot-notation)
* [Arrays in MongoDB](https://docs.mongodb.org/manual/tutorial/query-documents/#arrays)
* [Update Operators in MongoDB](https://docs.mongodb.org/manual/reference/operator/update/)

Both methods have its advantages, and disadvantages. It depends on the situation. Choose wisely.






