'''shell
mkdir project
cd project
mkdir shared
vagrant init django-base-v2.1
'''

'''shell
config.vm.network "forwarded_port", guest: 8000, host: 8080
config.vm.synced_folder "shared_folder", "/home/vagrant/shared_folder"
'''

'''shell
vagrant up
vagrant ssh
'''

'''shell
sudo apt-get update
sudo apt-get upgrade

git config --global user.email "severo.fabricio@gmail.com"
git config --global user.name "Severo"
git config --global push.default upstream

# Installing Oh-My-Zsh
sudo apt-get install zsh
wget https://github.com/robbyrussell/oh-my-zsh/raw/master/tools/install.sh -O - | zsh
chsh -s `which zsh`

# reboot

# Heroku Toolbelt (dependencies first)
sudo apt-get install python-dev python-psycopg2 libpq-dev
wget -qO- https://toolbelt.heroku.com/install-ubuntu.sh | sh

mkvirtualenv project
pip install django-toolbelt

pip freeze > requirements.txt

django_admin startproject project .
'''

CONFIGURE SETINGS
[http://www.marinamele.com/2013/12/how-to-set-django-app-on-heroku-part-i.html]
[https://devcenter.heroku.com/articles/django-assets]
[https://devcenter.heroku.com/articles/getting-started-with-python#provision-a-database]

'''shell
git init
git add .
git commmit -m "First commit"

# ... configure git

git push
heroku create
git push heroku master 

# migrations
heroku run bash

python manage.py migrate
'''
