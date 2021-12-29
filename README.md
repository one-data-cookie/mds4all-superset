# mds4all-superset

This is a visualisation layer of *Modern Data Stack for All* –
[Apache Superset](https://superset.apache.org/) hosted on
[Heroku](https://dashboard.heroku.com/). The ELT part of the stack
is to be found in [mds4all-elt](https://github.com/one-data-cookie/mds4all-elt).

## Setup

```shell
# Set up basics
$ pip install apache-superset
$ pip install psycopg2
$ pip freeze > requirements.txt # or just limit to installed above
$ echo "web: superset db upgrade" > Procfile
$ echo "python-3.8.2" > runtime.txt
# add configs to superset_config.py

# Generate secret key
$ python
>>> from cryptography import fernet
>>> fernet.Fernet.generate_key()

# Set up Heroku app
$ heroku login
$ heroku create mds4all-superset
$ heroku config:set SECRET_KEY=<SECRET_KEY>
$ heroku config:set PORT=<PORT>
$ heroku addons:create heroku-postgresql:hobby-dev
$ git push heroku main
$ heroku logs –-tail # check for errors

# Initialise Superset
$ heroku run bash
$ export FLASK_APP=superset
$ superset fab create-admin
$ superset init
$ exit

# Finalise app
$ echo 'web: gunicorn "superset.app:create_app()"' > Procfile
$ git push heroku main
$ heroku logs --tail # check for errors

# Open app
$ heroku open
# add pybigquery to requirements.txt for BigQuery connections
```

## Limitations
- Sleeps after 30 mins of inactivity
- 512 MB RAM
- Only 4 workers
- Running on Gunicorn server
- No cache (like Redis)
- Postgres database:
  - 10K rows
  - 20 MB
