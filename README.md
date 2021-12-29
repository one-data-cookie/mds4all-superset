# mds4all
Minimalistic and free **m**odern **d**ata **s**tack, hence **for all**.

- [`one-data-cookie/mds4all-elt`](https://github.com/one-data-cookie/mds4all-elt):
  - Warehousing: [Google BigQuery](https://cloud.google.com/bigquery/)
  - Orchestrating: [Github Actions](https://github.com/features/actions)
  - Ingesting: [Meltano](https://meltano.com/)
  - Transforming: [dbt](https://www.getdbt.com/)
  - Cataloging: [dbt docs](https://www.getdbt.com/docs/) on [GitHub Pages](https://pages.github.com/)

- [`one-data-cookie/mds4all-superset`](https://github.com/one-data-cookie/mds4all-superset):
  - Visualising: [Superset](https://superset.apache.org/) on [Heroku](https://dashboard.heroku.com/)

- `general`:
  - Code-editing: [VS Code](https://code.visualstudio.com/) with:
    - [BigQuery Runner](https://marketplace.visualstudio.com/items?itemName=minodisk.bigquery-runner)
    - [dbt Power User](https://marketplace.visualstudio.com/items?itemName=innoverio.vscode-dbt-power-user)
    - [dbt Language Server](https://marketplace.visualstudio.com/items?itemName=Fivetran.dbt-language-server)

# mds4all-superset
Visualisation layer of `mds4all`.

## Setup
```shell
# Set up basics
$ pip install apache-superset
$ pip install psycopg2
$ pip install pybigquery # for BigQuery connection
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
```

## Limitations
- Sleep after 30 mins of inactivity
- 512 MB RAM
- 4 workers
- Gunicorn server
- No cache (like Redis)
- Database storage with 10K rows and 20 MB

## Inspiration
- [`Chizzy-codes/NBA_Dashboard`](https://github.com/Chizzy-codes/NBA_Dashboard)
- [`dugjason/superset-on-heroku`](https://github.com/dugjason/superset-on-heroku)
- [`zi-nt/superset-on-heroku`](https://github.com/zi-nt/superset-on-heroku)
