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


