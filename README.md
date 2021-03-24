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
