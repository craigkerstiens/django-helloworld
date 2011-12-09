Hello World with Django
=======================

Prerequisites
-------------

* Python, virtualenv, pip, and the Heroku toolbelt as described in the Python quickstart
* An installed version of Postgres to test locally

Step 1 - Create your project
----------------------------

1. Start by making an empty top-level directory for the project::

	$ mkdir hellodjango && cd hellodjango

2. Create a pip requirements.txt file which declares our use of Django::

	Django==1.3
	psycopg2==2.4.2

3. Create a Virtualenv::

	$ virtualenv --no-site-packages .
	New python executable in ./bin/python
	Installing setuptools............done.

Before running pip (or your application), you’ll need to source the Virtualenv environment::

	$ source bin/activate

This will change your prompt to include the project name. (You must source the virtualenv environment for each terminal session where you wish to run your app.)

4. Install dependencies with pip::

	$ pip install -r requirements.txt
	  Downloading/unpacking psycopg2==2.4.2 (from -r requirements.txt (line 2))
  	  Downloading psycopg2-2.4.2.tar.gz (667Kb): 667Kb downloaded
  	  Running setup.py egg_info for package psycopg2
    	no previously-included directories found matching 'doc/src/_build'
	  Downloading/unpacking Django==1.3 (from -r requirements.txt (line 1))
  	  Downloading Django-1.3.tar.gz (6.5Mb): 6.5Mb downloaded
  	  Running setup.py egg_info for package Django
	  Installing collected packages: Django, psycopg2
	  ...
	  Successfully installed Django psycopg2
	  Cleaning up...

5. Now we can create a Django app in a subdirectory::

	$ django-admin.py startproject hellodjango

Note that we’ve packaged the app, placing the Django app in a subdirectory of our main project. This keeps our main Django app separate from Virtualenv artifacts and other Django apps we might want to add to the directory.

Step 2: Test your App Locally
-----------------------------

1. Test that the app runs locally by starting up the Django development webserver::

	$ python hellodjango/manage.py runserver
	Validating models...

	0 errors found
	Django version 1.3, using settings 'hellodjango.settings'
	Development server is running at http://127.0.0.1:8000/
	Quit the server with CONTROL-C.
	Ctrl-C to exit the server.

Step 3: Deploy the Web App to Heroku
------------------------------------

1. Commit to Git

Exclude Virtualenv artifacts from source control tracking by adding a .gitignore file::

	bin
	build
	include
	lib
	.Python
	*.pyc

2. Initialize a Git repo and commit the project (if you aren’t already tracking your project with Git)::

	$ git init
	Initialized empty Git repository in /Users/adam/hellodjango/.git/
	$ git add .
	$ git commit -m "my django app"
	[master (root-commit) 8c07531] my django app
	5 files changed, 184 insertions(+), 0 deletions(-)
	create mode 100644 .gitignore
	create mode 100644 hellodjango/__init__.py
	create mode 100644 hellodjango/manage.py
	create mode 100644 hellodjango/settings.py
	create mode 100644 hellodjango/urls.py
	create mode 100644 requirements.txt
	Deploy to Heroku

3. Create an app on the Cedar stack::

	$ heroku create --stack cedar
	Creating afternoon-sword-29... done, stack is cedar
	http://afternoon-sword-29.herokuapp.com/ | git@heroku.com:afternoon-sword-29.git
	Git remote heroku added

4. Deploy your app::

	$ git push heroku master
	Counting objects: 9, done.
	Delta compression using up to 4 threads.
	Compressing objects: 100% (6/6), done.
	Writing objects: 100% (9/9), 3.01 KiB, done.
	Total 9 (delta 0), reused 0 (delta 0)

	-----> Heroku receiving push
	-----> Python/Django app detected
	-----> Preparing virtualenv version 1.6.1
		New python executable in ./bin/python2.7
		Also creating executable in ./bin/python
		Installing setuptools............done.
		Installing pip...............done.
	-----> Byte-compiling code
	-----> Django settings injection
		Injecting code into hellodjango/settings.py to read from DATABASE_URL
	-----> Installing dependencies using pip version 1.0.1
	Downloading/unpacking Django==1.3 (from -r requirements.txt (line 1))
	...
		Successfully installed Django psycopg2
		Cleaning up...
	-----> Discovering process types
		Procfile declares types         -> (none)
		Default types for Python/Django ->web
	-----> Compiled slug size is 8.0MB
	-----> Launching... done, v3
		http://afternoon-sword-29.herokuapp.com deployed to Heroku

	To git@heroku.com:afternoon-sword-29.git
	* [new branch]      master -> master

5. Check to see that your web process is up::

	$ heroku ps
	Process       State               Command
	------------  ------------------  ------------------------------
	web.1         up for 4s           python hellodjango/manage.py r..

6. View your logs::

	$ heroku logs
	2011-08-27T07:58:00+00:00 heroku[web.1]: Starting process with command `python hellodjango/manage.py runserver 0.0.0.0:8642 --noreload`
	2011-08-27T07:58:00+00:00 app[web.1]: Validating models...
	2011-08-27T07:58:00+00:00 app[web.1]: 
	2011-08-27T07:58:00+00:00 app[web.1]: 0 errors found
	2011-08-27T07:58:00+00:00 app[web.1]: Django version 1.3, using settings 'hellodjango.settings'
	2011-08-27T07:58:00+00:00 app[web.1]: Development server is running at http://0.0.0.0:8642/
	2011-08-27T07:58:00+00:00 app[web.1]: Quit the server with CONTROL-C.
	2011-08-27T07:58:01+00:00 heroku[web.1]: State changed from starting to up

7. Finally, visit your app on the web::

	$ heroku open