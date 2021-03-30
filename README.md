### 1. Basic

Create file Name.py
and insert this code. 

```
from flask import Flask
app = Flask(__name__)

@app.route('/')
def hello_world():
    return 'Hello, World!'

```

to start server 
```
flask run
```

to change port and host:
```

port:
export FLASK_RUN_PORT=8499

change host to global (all can be see open this server):
flask run --host=0.0.0.0
```

If you change somthing on the server it can't be use immediately, u need to reload it by yourself 
BUT, u can use this to immediately auto reload:
```
export FLASK_ENV=development
```


let's try some new stuff:

this code will return your Label by using simple flask variables:
```
from flask import Flask
from markupsafe import escape
app = Flask(__name__)

@app.route('/home/')
def hello_world():
    return 'Hello, World!'
    
@app.route('/user/<username>')
def show_user_profile(username):
    # show the user profile for that user
    return 'User %s' % escape(username)
```
so this return "Name" by IP:Port/Name

and we can use "convert": <converter: ... > if we clearly assured of type 
```
@app.route('/post/<int:post_id>')
def show_post(post_id):
    # show the post with the given id, the id is an integer
    return 'Post %d' % post_id
```


we can detect a different http methonds by use this code:
```
@app.route('/login', methods=['GET', 'POST'])
def login():
    if request.method == 'POST':
        return do_the_login() // use post
    else:
        return show_the_login_form() // use get
```

If u want to create custom html page with some parametrs -> read about templats


### Base data

I'm really like Postgress, let's connect that to out server

1. find / -name "postgresql.conf"
in command, and get path of this file.
that change in this file:
```
#listen_addresses = '*'	
port = 1234 (your open port)
```
save
2. you need to set password to postgress acc:
```
sudo -u postgres psql
```
and in new window print:
```
ALTER USER postgres PASSWORD 'yourpass';
CREATE DATABASE my_database;
```


3. add some code in your app.py
```
from flask import Flask
from markupsafe import escape
from flask import request
from flask import render_template
from flask_sqlalchemy import SQLAlchemy
from flask_migrate import Migrate

app = Flask(__name__)

POSTGRES = {
    'user': 'postgres',
    'pw': 'yourpass',
    'db': 'my_database',
    'host': 'localhost',
    'port': 'yourport',
}

app.config['SQLALCHEMY_DATABASE_URI'] = 'postgresql://%(user)s:\
%(pw)s@%(host)s:%(port)s/%(db)s' % POSTGRES

app.config['SQLALCHEMY_TRACK_MODIFICATIONS'] = True
db = SQLAlchemy(app)
migrate = Migrate(app, db)


@app.route('/home/')
def hello_world():
    return 'Hello, World!'

@app.route('/user/<username>')
def show_user_profile(username):
    # show the user profile for that user
    return 'User %s' % escape(username)

@app.route('/hello/<name>')
def hello(name=None):
    return render_template('hello.html', name=name)

@app.route("/me")
def me_api():
    #user = get_current_user()
    return {
        "username": "user.username",
        "theme": "user.theme",
    }

class CarsModel(db.Model):
    __tablename__ = 'cars'

    id = db.Column(db.Integer, primary_key=True)
    name = db.Column(db.String())
    model = db.Column(db.String())
    doors = db.Column(db.Integer())

    def __init__(self, name, model, doors):
        self.name = name
        self.model = model
        self.doors = doors

    def __repr__(self):
        return f"<Car {self.name}>"

```


### 2. Admin panel

let's import 
```
pip install flask-admin
```

and then get dependencies to main file app.py
```
from flask_admin import Admin
app.config['FLASK_ADMIN_SWATCH'] = 'cerulean'
admin = Admin(app, name='microbase', template_mode='bootstrap3')
```
https://flask-admin.readthedocs.io/en/latest/
