### orator
---
https://github.com/sdispater/orator

```py
from oractor import DatabaseManager, Model

config = {
  'mysql': {
    'driver': 'mysql',
    'host': 'localhost',
    'database': 'database',
    'user': 'root',
    'password': '',
    'prefix': ''
  }
}

db = DatabaseManager(config)
Model.set_connection_resolver(db)

class User(Model):
  pass

class User(Model):
  __table__ = 'my_users'


users = User.all()

user = User.find(1)
print(user.name)


users = User.where('votes', '>', 100).take(10).get()
for user in users:
  print(user.name)

count = User.where('votes', '>', 100).count()

users = User.where_raw('age > ? and votes = 100', [25]).get()

for user in User.chunk(100):
  for user in users:
  
user = User.on('connection-name').find(1)

user = User.on_write_connection().find(1)

class User(Model):
  __fillable__ = ['first_name', 'last_name', 'email']

class User(Model):
  __guarded__ = ['id', 'password']

__guarded__ = ['*']

user = User()
user.name = 'John'
user.save()

inserted_id = user.id

user = User.create(name='John')
user = User.first_or_create(name='John')
user = User.first_or_new(name='John')

user = User.find(1)
user.name = 'Foo'
user.save()

affected_rows = User.where('votes', '>', 100).update(status=2)
user = User.find(1)
user.destroy(1, 2, 3)

affected_rows = User.where('votes', '>' 100).delete()

user.touch()

class User(Model):
  __timestamps__ = False
  
class User(Model):
  def get_date_format():
    return 'DD-MM-YY'

user = User.with_('roles').first()
return user.to_dict()

return User.all().serailize()

return User.find(1).to_json()

users = db.table('users').get()

for user in users:
  print(user['name'])

for users in db.table('users').chunk(100):
  for user in users:
  
user = db.table('users').where('name', 'John').first()
print(user['name'])

user = db.table('users').where('name', 'John').plunk('name')

roles = db.table('roles').lists('title')

roles = db.table('roles').lists('title', 'name')

users = db.table('users').select('name', 'email').get()
users = db.table('users').distinct().get()
users = db.table('users').select('name as user_name').get()

query = db.table('users').select()
users = query.add_select('age').get

users = db.table('users').where('age', '>', 25).get()

users = db.table('users').where('age', '>', 25).or_where('name', 'John').get()

users = db.table('users').where_between('age', [25, 35]).get()

users = db.table('users').where_not_between('age', [25, 35]).get()

users = db.table('users').where_in('id', [1, 2, 3]).get()
users = db.table('users').where_not_in('id', [1, 2, 3]).get()

users = db.table('users').where_null('updated_at').get()

query = db.table('users').order_by('name', 'desc')
query = query.group_by('count')
query = query.having('count', '>', 100)

users = query.get()

users = db.table('users').skip(10).take(5).get()
users = db.table('users').offset(10).limit(5).get()

db.table('users') \
  .join('contacts', 'users.id', '=', 'contacts.user_id') \
  .join('orders', 'users.id', '=', 'orders.user_id') \
  .select('users.id', 'contacts.phont', 'orders.price') \
  .get()

db.table('users').left_join('posts', 'users.id', '=', 'posts.usrs_id').get()

clause = JoinClause().on('users.id', 'contacts.user_id').where('contacts.user_id').or_on(...)
db.table('users').join(clause).get()

clasuse = JoinClause('contacts').on('users.id', '=', 'contacts.user_id').where('contacts.user_id', '>', 5)
db.table('users').join(clause).get()

db.table('users') \
  .where('name', '=', 'John') \
  .or_where(
    db.query().where('votes', '>', 100).where('titie', '!=', 'admin')
  ).get()

db.table('users').where_exists(
  db.table('orders').select(db.raw(1)).where_raw('order.user_id = users.id')
)

users = db.table('users').count()
price = db.table('orders').max('price')
price = db.table('orders').min('price')
price = db.table('orders').avg('price')
total = db.table('users').sum('votes')

db.table('users') \
  .select(db.raw('count(*) as user_count, status')) \
  .where('status', '!=', 1) \
  .group_by('status') \
  .get()

db.table('users').insert(email='foo@bar.com', votes=0)

db.table('users').insert({
  'email': 'foo@bar.com',
  'votes': 0
})

id = db.table('users').insert_get_id({
  'emaill': 'foo@bar.com',
  'votes': 0
})

db.table('users').where('id', 1).update(voets=1)
db.table('users').where('id', 1).update({'votes': 1})

db.table('users').increment('votes')
db.table('users').increment('votes', 5)
db.table('users').decrement('votes')
db.table('users').decrement('votes', 5)

db.table('users').increment('votes', 1, name='John')

db.table('users').where('age', '<', 25).delete()

db.table('users').delete()

db.table('users').truncate()

first = db.table('users').where_null('first_name')
users = db.table('users').where_null('last_name').union(first).get()

config = {
  'mysql': {
    'read': {
      'host': '192.168.1.1'
    },
    'write': {
      'host': '192.168.1.2'
    },
    'dirver': 'mysql',
    'database': 'database',
    'user': 'root',
    'password': '',
    'prefix': ''
  }
}

with db.transaction():
  db.table('users').update({votes: 1})
  db.table('posts').delete()

db.begin_transaction()
db.rollback()
db.commit()

users = db.connection('foo').table('users').get()
db.connection().get_connection()
db.reconnect('foo')
db.disconnect('foo')
```

```sh
pip install orator

SELECT * FROM users WHERE name = 'John' OR (votes > 100 AND title != 'Admin')
```

```
```


