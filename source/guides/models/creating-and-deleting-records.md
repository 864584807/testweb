You can create records by calling the `createRecord` method on the store.

```js
store.createRecord('post', {
  title: 'Rails is Omakase',
  body: 'Lorem ipsum'
});
```

The store object is available in controllers and routes using `this.store`.

Although `createRecord` is fairly straightforward, the only thing to watch out for
is that you cannot assign a promise as a relationship, currently.

For example, if you wanted to set the author of a post, this would **not** work
 if the `user` with id wasn't already loaded into the store:

```js
var store = this.store;

store.createRecord('post', {
  title: 'Rails is Omakase',
  body: 'Lorem ipsum',
  author: store.find('user', 1)
});
```

However, you can easily get by this by setting the relationship afterwards:

```js
var store = this.store;

store.createRecord('post', {
  title: 'Rails is Omakase',
  body: 'Lorem ipsum'
});

post.set('author', store.find('user', 1))
```

### Deleting Records

Deleting records is just as straightforward as creating records. Just call `.deleteRecord()`
on any instace of `DS.Model`. This flags the record as `isDeleted` and thus removes
it from `.all()` queries on the `store`. The deletion can then be persisted using `model.save()`.
Alternatively, you can use the `destroyRecord` method to delete and persist at the same time.

```js
var post = store.find('post', 1);

post.deleteRecord();

post.get('isDeleted');
// => true

post.save();
// => DELETE to /posts/1

// OR

var post = store.find('post', 2);

post.destroyRecord();
// => DELETE to /posts/1
```