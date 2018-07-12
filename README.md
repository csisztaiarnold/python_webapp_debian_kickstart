# Python WebApp (Flask) Kickstart on Debian

![flask](https://i.imgur.com/N22AHP6.png)

Install Python's development files and pip, Python'spackage management system.

~~~~
sudo apt-get install python-pip python-dev
~~~~

Install `virtualenv`. It is a tool to create isolated Python environments.

~~~~
sudo pip install virtualenv
~~~~

Create a folder where your project files will remain, and change your directory to the newly created one.

~~~~
mkdir ~/myproject
cd ~/myproject
~~~~

Inside the project's folder, create the project's virtual environment. This will install `python`, `setuptools`, `pip`, `wheel`, etc. into your dev environment.

<pre>
virtualenv <b>env</b>
</pre>

Now activate the virtual environment. This will change the prompt into something like `(env)user@host:~/myproject$.`

<pre>
source <b>env</b>/bin/activate
</pre>

Inside the virtual environmnent, install `flask` (a microframework for Python) and `uwsgi` (an http server).

~~~~
pip install uwsgi flask
~~~~

Create a new python file named `myproject.py` and put it into the project's root (`~/myproject/`).

~~~~
rom flask import Flask
application = Flask(__name__)

@application.route("/")
def hello():
    return "Hello World!"

if __name__ == "__main__":
    application.run(host='0.0.0.0')
~~~~

If everything went right, a `python myproject.py` command will start the application, most likely on port 5000, but check the terminal output after running the command to see the specific address (look for the line `Running on http://0.0.0.0:5000/` or similar). Visit the address, and voilà, the app should be there.

Now in the same folder, create a WSGI entry point named `wsgi.py`. The purpose of this file is to tell the uWSGI server how to communicate with the app.

~~~~
from myproject import application

if __name__ == "__main__":
    application.run()
~~~~

After this, start the application with the following command:

~~~~
uwsgi --socket 0.0.0.0:8000 --protocol=http -w wsgi
~~~~

Of course, after this, you could reach your app on port **8000**

**NOTE**: **never expose a socket speaking the uwsgi protocol to the public network**, unless you know what you are doing.
If you'd like to expose your site to public, use the `--http` or `--http-socket` options instead of `--socket`. Anyhow, the best way to expose the site is to use `nginx`, `apache` or the `uWSGI routers` as a proxy. Basically, if you plan to expose uWSGI directly to the public, use --http, if you want to proxy it behind a webserver speaking http with backends, use --http-socket.

## Todo

Using `nginx` as a proxy to expose the app to public.