# Following [https://aryaboudaie.com](https://aryaboudaie.com/python/technical/educational/web/flask/2018/10/17/flask.html)'s tutorial

# Install Flask

```
pip3 install flask   # linux
pip install flask    # windows
```

# Create flask environemnt

```
virtualenv flaskapp
cd flaskapp
. bin/activate

```

# In Python - Hello World in Flask

Save under `flask_test.py`:
```
from flask import Flask

app = Flask(__name__)

@app.route('/')
def hello_world():
    return 'Hello World!'

if __name__ == '__main__':
    app.run(host='0.0.0.0')
```

Do then:
```
python flask_test.py
```
It will say Running on http://0.0.0.0:5000/

Put into browser "localhost:5000"

Click to rectangle-arrow symbol next to address.

Add another page:
```
@app.route('/test')
def testing():
    return 'test'
```

Get user input:
```
@app.route('/hello/<name>') # the variable <name> will passed to function!
def hello_name(name):
    return 'Hello ' + name + '!'

@app.route('/square/<int:num>') # casts as int! and names variable 'num'
def f(num):
    return str(num**2)

def factors(num):
    return [x for x in range(1, num+1) if num % x == 0]

@app.route('/factors/<int:num>')

```

In browser "localhost:5000/hello/arya" will pass `arya` as `name`
and creates there `Hello arya!`.

Such `<var>` values are strings.
But we can by `<int:var>` manually convert. 

So for any function in Python, we can create a webinterface.

# Make pages nice

```
<h1>The factors of 10 are:</h1>
<ul>
  <li>1</li>
  <li>2</li>
  <li>5</li>
  <li>10</li>
</ul>
```

For quick rendering, use [jsfifle.net](https://jsfiddle.net/).

```
@app.route('/factors_raw/<int:num>')
def factors_display_raw_html(num):
    factors_list = factors(int(num))
    html = "<h1>The fators of " + str(n) + " are</h1>" + "\n" + \
           "<ul>"
    for f in factors_list:
        html += "<li>" + str(f) + "</li>" + "\n"
    html += "</ul> </body>
    return html
```

# HTML Templates

use HTML templating engine `Jinja2`

In `factors.html`:
```
<h1>The factors of {{ number }} are:</h1>
<ul>
  {% for factor in factors %}
  <li>{{ factor }}</li>
  {% endfor %}
</ul>
```

```
from flask import render_template

@app.route('/factors/<int:n>')
def fators_display(n):
    return render_template(
            "factors.html", # name of template
            number=n, # value for `number` in template
            factors = factors(n) # value for `factors` in template
    )
```

# Current state

```
from flask import Flask, render_template
import random
app = Flask(__name__)

def factors(num):
    return [x for x in range(1, num+1) if num%x == 0]

@app.route('/')
def home():
    n = random.randint(2, 10000)
    return render_template(
        "index.html",
        rndom_num=n,)

@app.route('/factors/<int:n>')
def factors_dispaly(n):
    return render_template(
        "factors.html",
        number=n
        factors=factors(n)
    )

if __name__ == '__main__':
    app.run(host='0.0.0.0')
```
[see template files in repl.it](https://repl.it/@misingnoglic/factors-with-Jinja2-Template)
`dummy.py` file is just because of a bug in repl.it - a second empty Python file necessary.


# Add state using `session`

```
from flask import Flas, render_template, session

app = Flask(__name__)

# add these two extra lines
app.secret_key = 'your secret'             # in production really secret key needed!
app.config['SESSION_TYPE'] = 'filesystem'
```
Usage of secret key [here](https://stackoverflow.com/questions/22463939/demystify-flask-app-secret-key) and [here](https://www.reddit.com/r/flask/comments/5l2gmf/af_eli5_how_sessions_work_in_flask/).

After that, you can use session obj like a Python dictionary.

```
@app.route('/')
def home():
    x = session.get('x', None)
    if not x:
        sessiion['x'] = 1
    elif x >= 10:
        session.clear()
        return "Session Cleard"
    else:
        session['x'] += 1
        return str(session['x']
```
[Flask mega-tutorial](https://blog.miguelgrinberg.com/post/the-flask-mega-tutorial-part-i-hello-world)
[Flask official quickstart](http://flask.pocoo.org/docs/1.0/quickstart/)
[Flask official tutorial](http://flask.pocoo.org/docs/1.0/tutorial/)
[Flask official documentation](http://flask.pocoo.org/docs/1.0/)

[Seth Rait's how internet works Part1](http://flask.pocoo.org/docs/1.0/)
[Part2](https://sethrait.com/How-the-Internet-Works-Part-II.html)

[arya@aryaboudaie.com]
[https://twitter.com/misingnoglic](https://twitter.com/misingnoglic)





