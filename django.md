# Working on Django with Heroku

Preparation of the development environment according to my [Workstation](worstation.md)

```shell
mkdir project_name
cd project_name
mkdir shared
vagrant init django-base-v2.1
```

Edit the `Vagrantfile` and add those two lines for portforwarding and a shared folder:

```ruby
config.vm.network "forwarded_port", guest: 8000, host: 8080
config.vm.synced_folder "shared", "/home/vagrant/shared"
```

Startup the VM:

```shell
vagrant up
vagrant ssh
```

Before starting to work, update the system, configure git and install [Heroku Toolbelt](https://toolbelt.heroku.com/), [zsh](http://www.zsh.org/) and [oh-my-zsh](http://ohmyz.sh/):

```shell
# Updating the system
sudo apt-get update
sudo apt-get upgrade

# Git configuration
git config --global user.email "severo.fabricio@gmail.com"
git config --global user.name "Severo"
git config --global push.default upstream

# Heroku Toolbelt (and dependencies)
sudo apt-get install python-dev python-psycopg2 libpq-dev
wget -qO- https://toolbelt.heroku.com/install-ubuntu.sh | sh

# Installing Oh-My-Zsh
sudo apt-get install zsh
wget https://github.com/robbyrussell/oh-my-zsh/raw/master/tools/install.sh -O - | zsh
chsh -s `which zsh`
# Reboot to apply changes and start with zsh
```

Starting the project:

```shell
# Working on the shared folder to edit files on Windows
cd shared

# I do create the virtualenv with the name "shared" so when i enter the folder it will automatically activate (oh-my-zsh plugin)
mkvirtualenv shared

# Install Django, psycopg2, gunicorn, dj-database-url, dj-static
pip install django-toolbelt

# Freeze the requirements to Heroku
pip freeze > requirements.txt

# Start project (Don't forget the dot at the end to create files at the actual directory)
django_admin startproject project_name .
mkdir staticfiles
mkdir static
touch static/empty
```

Some configuration i found at
["Python, Science and Marketing"](http://www.marinamele.com/2013/12/how-to-set-django-app-on-heroku-part-i.html),
[Heroku Dev Center (Static files)](https://devcenter.heroku.com/articles/django-assets)
and [Heroku Dev Center (Python tutorial)](https://devcenter.heroku.com/articles/getting-started-with-python#provision-a-database):

Edit your `postactivate` virtualenv hook (`vim $WORKON_HOME/shared/bin/postactivate`) and make sure to restart the virtual env to get this working:
```shell
DJANGO_SETTINGS_MODULE="project_name.dev_settings"
export DJANGO_SETTINGS_MODULE

production () {
  heroku maintenance:on;
  git push heroku master;
  heroku run python manage.py migrate;
  heroku maintenance:off;
}
```

Edit your `settings.py` and add the following configuration:

```python
import dj_database_url
# ...
ALLOWED_HOSTS = ['project_name.herokuapp.com']
# ...
STATIC_ROOT = 'staticfiles'
STATIC_URL = '/static/'

STATICFILES_DIRS = (
    os.path.join(BASE_DIR, 'static'),
)

SECURE_PROXY_SSL_HEADER = ('HTTP_X_FORWARDED_PROTO', 'https')

DATABASES['default'] = dj_database_url.config()
DEBUG = False
TEMPLATE_DEBUG = False
```

Create a `dev_settings.py` together with the `settings.py` with the following content:
```python
# -*- coding: utf-8 -*-
from .settings import *
DEBUG = True
TEMPLATE_DEBUG = DEBUG
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.sqlite3',
        'NAME': os.path.join(BASE_DIR, 'db.sqlite3'),
    }
}
```

Edit your `wsgi.py` and add the following configuration:

```python
import os
os.environ.setdefault("DJANGO_SETTINGS_MODULE", "project_name.settings")

from django.core.wsgi import get_wsgi_application
from dj_static import Cling

application = Cling(get_wsgi_application())
```

Create a `Procfile` with the following content:
```shell
web: gunicorn financial.wsgi --log-file -
```

Create a `.gitignore` with the following content:
```shell
*.pyc
*.sqlite3
internal_docs/
staticfiles/
```

When you are ready to make your first commit/deploy:

```shell
git init
git add .
git commmit -m "First commit"

# Configure your git remote
git remote add origin git@github.com:fsevero/project_name.git
git push -u origin master

git push
heroku login
heroku create project_name
git push heroku master

# If not exists, create a folder staticfiles
heroku run mkdir staticfiles

# migrations
heroku run python manage.py migrate
```

Whenever you do some changes and want to publish:

```shell
git add .
git commmit -m "commit mesage with change description"

git push

# The function 'production' will trigger the snippet configured at the 'postactivate' hook of the virtual env
production
```
