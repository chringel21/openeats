# OpenEats Project

**OpenEats** is a continuation from [qgriffith/OpenEats](https://github.com/qgriffith/OpenEats) and [pando85/openeats](https://github.com/pando85/openeats).

## Changes
* removed docker structure for hosting on [uberspace](https://uberspace.de)

## Essential steps
Make sure Virtualenv is ready

``` shell
pip2.7 install virtualenv --user
pip2.7 install virtualenvwrapper --user
```

Add some lines to `.bashrc`

``` 
export WORKON_HOME=~/Envs
export VIRTUALENVWRAPPER_PYTHON=/usr/local/bin/python2.7
source bin/virtualenvwrapper.sh
```

``` shell
source .bashrc
mkdir -p $WORKON_HOME
```

Make virtual environment

``` shell
mkvirtualenv openeats --no-site-packages
workon openeats
```

Clone repository, install required packages

``` shell
git clone https://github.com/pando85/openeats.git
pip2.7 install -r openeats/requirements.txt
```

*(Adapt or skip this, if you want to use another database like MySQL or PostgreSQL).*

Create an empty sqlite3 database file and change `settings.py` accordingly.

``` shell
sqlite3 ~/openeats/openeats/openeats.db ".databases"
```

Make migrations, fill up the database with sample data, create static files and create superuser. Apply some minor fixtures

``` shell
./manage.py makemigrations
./manage.py migrate
./manage.py collectstatic --noinput --clear
./manage.py createsuperuser

./manage.py loaddata openeats/accounts/fixtures/test_user_data.json
./manage.py loaddata openeats/list/fixtures/list_test_data.json
./manage.py loaddata openeats/list/fixtures/aisle_data.json  
./manage.py loaddata openeats/accounts/fixtures/test_friend_data.json
./manage.py loaddata openeats/recipe_groups/fixtures/course_data.json
./manage.py loaddata openeats/recipe_groups/fixtures/cuisine_data.json
./manage.py loaddata openeats/recipe/fixtures/recipe_data.json
./manage.py loaddata openeats/ingredient/fixtures/ing_data.json
```

## Making changes to languages and locale
Adapt `openeats/locale/de/LC_MESSAGES/django.po`, then use

``` shell
./manage.py makemessages -l de
./manage.py compilemessages -f
```

Restart service!
