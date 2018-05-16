# Python

# What makes Python unique
* Meaningful white space. In JS, we used curly braces. In Python, we use indentation/white space
* Variable types are inferred rather than declared
* Tons of communities and libraries
* Easter Eggs. It's named after Monty Python

## Installing Python
```bash
pip3 install ipython
 # pip3 is the installer for python
ipython
 # this is the python repl
import this
 # prints out the zen of python
import __hello__
 # prints hello world
import antigravity
 # opens an XKCD
```

## Libraries
Import libraries using ```import``` in ipython

## Python syntax tips
* variables_use_underscore
* Do ```6/2``` will return a number with 1 decimal but ```6//3``` will round to an integer
* ```\n``` gives a return
* ```print()``` is the console.log

## Basic functions
```python
print(variable_name)
str(number)
# returns a string
int(number)
# returns an integer
input()
# opens a user repl to input stuff
```

### String Concatenation
```python
full_name = 'Jason' + 'Toups'
```

### String Format Method
```python
In [14]: person1 = 'Jason'

In [15]: person2 = 'Two'

In [16]: occupation = 'Developer'

In [17]: '{0} is a {1}. {2} is a {1} as well'.format(person1, occupation, person2)
Out[17]: 'Jason is a Developer. Two is a Developer as well'
```

### F string
```python
In [18]: f'I am teaching {class_number}'
Out[18]: 'I am teaching 22'
```
'f-string' is a shorthand for the format method

### Booleans
```python
True
False
None 
# this is Null

'Jason' == 'Jason'
# 2 equal signs
# will return True
```

### Conditionals
```python
this and this
this or this
this not this
class_number > 20 and name == 'Jason'
```

### Control Flow
```python
In [29]: if height < 4:
    ...:     print("you can't come on the rollercoaster")
    ...: elif height < 7:
    ...:     print("Welcome!")
    ...: else:
    ...:     print("too tall")
    ...:
```
The indentation is somewhat automatic for us. Also, use the ```:``` after each ```if, elif, or else```

### Create a Python file
```bash
$ touch script.py
$ python3 script.py
```

### Lists (arrays)
```python
numbers = [1, 2, 3]

numbers.append(4)
# pushes 4 to the list

numbers.pop()
# returns the last item and removes it from the list

[1,2,3] + [4,5,6]
returns [1, 2, 3, 4, 5, 6]

numbers.extend([4,5,6])
# adds 4, 5, 6 to the end as individual items in the list instead of as a whole list 

numbers.remove(2)
returns [1, 3, 4, 5, 6]
# removes the number 2 from the list

sorted(numbers)
# sorts in alphabetic or numeric order
```

### Exercises
```python
2 ** 3 
# 2^3
```

### Functions in Python
```python
def double(number):
    number * 2
```

### List Comprehension (like Map)
We can use this to map. It's like running a function on each item in a list based on certain criteria. Pretty nifty. 

```python
listy = [1,2,3,4,5,6,7,8,9]

[number for  number in listy ]
# returns [1, 2, 3, 4, 5, 6, 7, 8, 9]

[number for number in listy if number % 2 == 0 ]
# returns [2, 4, 6, 8]
# this is sort of like a conditional for loop

string_nums = ['1', '2', '3']
[int(a) for a in string_nums]
# ths is pretty similar to a map function in JavaScript

def string_to_int_list(s):
    return [int(n) for n in s.split(",") if n]
```

## Setting Up Django
### Set up a Virtual Environment
```bash
$ pip install virtualenv
$ virtualenv .env -p python3
$ source .env/bin/activate
$ deactivate 
# ends the virtual environment. Closing works too
```
Python package installations are default global. The issue can come in when we're trying to use different versions of python or django. So we use a virtual environment which has the dependencies installed within it. These are like creating a different version of python local to your project.

### Install Django
```bash
$ pip install Django==2.0.5
$ pip install psycopg2
$ pip freeze > requirements.txt
$ cat requirements.txt
Django==2.0.5
pytz==2018.4
```
Be sure to re-run pip freeze > requirements.txt whenever you install a new dependency

### Start Django
```bash
$ django-admin startproject tunr_django
# sets up our django defaulots
$ cd tunr_django
$ django-admin startapp tunr
# sets up the starting files. You can have multiple set up in here
```
tunr_django is the new file created. Start project is a command. Manage.py will run our application

Files:
* tunr_django/urls: routes
* tunr_django/settings: middleware, templating language (check out Djinja), databases, security, authentication, static

### Setting up Postgres
From anywhere:
```bash
psql
CREATE DATABASE tunr;
CREATE USER tunruser WITH PASSWORD 'tunr';
GRANT ALL PRIVILEGES ON DATABASE tunr TO tunruser;
```
Go to tunr_django/settings.py
```python
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.postgresql_psycopg2',
        'NAME': 'tunr',
        'USER': 'tunruser',
        'PASSWORD': 'tunr',
        'HOST': 'localhost'
    }
}

INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'tunr'
]
# add 'tunr' to the Installed_Apps list
```
### Launching the app
Go to the directory with manage.py:
```bash
$ python manage.py runserver
```

### Defining Models
Go to models.py:
```python
from django.db import models

# Create your models here.
class Arist(models.Model):
    name = models.CharField(max_length = 100)
    nationality = models.CharField(max_length = 100)
    photo_url = models.TextField()
    
    def __str__(self):
        return self.name
        # This allows us to print the object and get something meaningful back

class Song(models.Model):
    artist = models.ForeignKey(Artist, on_delete=models.CASCADE, related_name='songs')
    # first argument is the other Model, on_delete it removes everything, pluralized name is songs
    title = models.CharField(max_length = 100, default = '')
    album = models.CharField(max_length = 100, default = '')
    preview_url = models.CharField(max_length = 100, default = '')

    def __str__(self):
        return self.title
```
null=True, blank=True works to make something optional

In the tunr_django folder
```bash
$ python manage.py makemigrations
# this creates a migrations folder and file. Don't mess with this
$ pythong manage.py migrate
# this sends the model to the Postgres server
```
You can view these in  ```postico```, which is a simple GUI for PostgreSQL

### Admin
```bash
$ python manage.py createsuperuser
username: jtoups
email: jason.p.toups@gmail.com
password:
```

In the admin.py file:
```python
from .models import Artist
admin.site.register(Artist)
```

Then, go to bash and run the server:
```bash
$ python manage.py runserver
```

Go to localhost:8000/admin

### Django Extensions and Shell (Django ORM)
In tunr_django
```bash
pip install django-extensions
```

In settings.py
```python
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'tunr',
    'django_extensions'
]
```

Back in the terminal:
```bash
$ pip install shell_plus
$ pip install ipython
$ python manage.py shell_plus --ipython
# creates a nicer shell
exit()
# escape
```
This is helpful for debugging. It's like an ipython console for our PostgreSQL server

In the iPython:
```bash
Artist.objects.all()
#  returns <QuerySet [<Artist: Beyonce>, <Artist: Ciara>]>
list(Artist.objects.all())
  # returns it as a list
Artist.objects.get(id=1)
  # returns the first item
Artist.objects.get(name="Beyonce")
  # returns the item Beyonce as name

chaka = Artist(name="Chaka Khan", photo_url="http://placebear.com/200/200", nationality="American")
  # creates a new object of class Artist

chaka.save()
  # saves that to the database

new_song = Song(title="Blue", album="Beyonce", preview_url="http://test", artist_id=1)
  # artist_id=1 can also be artist=chaka, but it has to refer to a variable

Artist.objects.filter(name__startswith='B')
  # returns all the artists whose name column values start with C

Song.objects.exclude(artist_id=1)
  # returns the songs that do not have an artist id 1
```

## MVT Framework
Model-View-Templates is as to Model-Controller-View. So the View is basically the Controller now. 

In the views file:
```python
def artist_list(request):
    artists = Artist.objects.all()
    return render(request, 'tunr/artist_list.html', {'artists': artists})

def song_list(request):
    songs = Song.objects.all()
    return render(request, 'tunr/song_list.html', {'songs': songs})

```

In the urls.py file in tunr_django (config folder)
```python
from django.conf.urls import include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('', include('tunr.urls'))
]
```

Create a urls.py file in the tunr folder
```python
from django.urls import path
from . import views

urlpatterns = [
    path('', views.artist_list, name='artist_list'),
    path('songs', views.song_list, name='song_list'),
    path('artists/<int:id>', views.artist_detail, name='artist_detail')
]
```

Now create a templates folder in the tunr directory. Create a tunr directory within that. And within that, create an artist_list.html
```html
<h2>Artists <a href="#">(+)</a> </h2>

<ul>
  {% for artist in artists %}
    <li>
      <a href="{% url 'artist_detail' id=artist.id %}">{{artist.name}}</a>
    </li>
  {% endfor %}
</ul>
  <!-- Single brackets are not rendered while double brackets are -->
</ul>
```

You also need to create a base.html file which will then pass in the different blocks
```html
{% load staticfiles %}
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <link rel="stylesheet" href="{% static 'css/tunr.css' %}">
  <title>Tunr</title>
</head>
<body>
  <h1>Tun.r</h1>
  <nav>
    <a href="/songs">Songs</a>
    <a href="/">Artists</a>
  </nav>
  {% block content %}
  {% endblock %}
</body>
</html>
```
Then, you need a static and css file. Create a folder called static in the tunr app directory, and then create a css folder. 

### Forms in Django
Create a forms.py file in the tunr app directory
```python
class ArtistForm(forms.ModelForm):
    class Meta: 
        model = Artist
        fields = ('name', 'photo_url', 'nationality',)
        # this is a tuple. It's a list that's immutable
```

Then add to the views:
```python
def artist_create(request):
    if request.method == 'POST':
        form = ArtistForm(request.POST)
        if form.is_valid():
            artist = form.save()
            return redirect('artist_detail', id=artist.id)
    else:
        form = ArtistForm()
    return render(request, 'tunr/artist_create.html', {'form': form}) 
```

Then create a view called artist_create
```html
{% extends 'tunr/base.html' %}
{% block content %}
<h2>New Artist</h2>
<form method="post" class="artist-form">
  {% csrf_token %}
  {{form.as_p}}
  <button type="submit" class="save btn btn-default">Save</button>
</form>

{% endblock %}
```

Lastly, add a URL which will point to this form:
```python
    path('artist/new', views.artist_create, name='artist_create')
```

## Steps
1. Create the environment
2. Install dependencies
3. Start Project -> cd project -> startapp
4. Set-up the database on psql and django
5. Add app to Installed apps
6. Write models
7. Make migrations -> migrate
8. Superuser
9. Views templating

## Seeding data
Ways to seed the database:
1. Go into admin console and add manually
2. Write a python script. Loop through rows, and columns. Insert into the database
3. COPY is a SQL command. So go into psql and can run a command
````\COPY players(name,age,team,games,points) FROM 'data.csv' WITH (FORMAT csv);````
https://www.postgresql.org/docs/8.4/static/sql-copy.html 
4. You can also execute SQL commands from Python. Use raw query? https://docs.djangoproject.com/en/2.0/ref/models/querysets/#raw

## REST Framework
Setting up Django REST Framework
```bash
start virtual environment
pip install djangorestframework
pip freeze > requirements.txt
```

In settings.py
```python
# add rest_framework to installed apps:
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'tunr',
    'rest_framework',
    'django_extensions'
]

# add REST_FRAMEWORK
REST_FRAMEWORK = {
    'DEFAULT_PERMISSION_CLASSES': [
        'rest_framework.permissions.DjangoModelPermissionsOrAnonReadOnly'
    ]
}
```
Note that the above uses an authentication layer on the API. Does not allowed anon users to edit  

In the global urls file:
```python
from django.contrib.auth import views as auth_views

urlpatterns = [
    path('admin/', admin.site.urls),
    path('', include('tunr.urls')),
    path('api-auth', include('rest_framework.urls'), namespace='rest_framework')
]
```

Create a serializers.py file in the app:
```python
from rest_framework import serializers
from .models import Artist, Song

class ArtistSerializer(serializers.HyperlinkedModelSerializer):
    songs = serializers.HyperlinkedRelatedField(
        view_name = 'song-detail',
        many=True,
        read_only=True
    )
    class Meta:
        model = Artist
        fields = ('id', 'photo_url', 'nationality', 'name', 'songs',)

class SongSerializer(serializers.HyperlinkedModelSerializer):
    artist = serializers.HyperlinkedRelatedField(
        view_name='artist_detail',
        read_only=True
    )
    class Meta:
        model = Song
        fields = ('artist', 'title', 'album', 'preview_url',)
```

In the Views file:
```python
from rest_framework import generics
from .serializers import ArtistSerializer, SongSerializer
from .models import Artist, Song

class ArtistList(generics.ListCreateAPIView):
    queryset = Artist.objects.all()
    serializer_class = ArtistSerializer

class ArtistDetail(generics.RetrieveUpdateDestroyAPIView):
    queryset = Artist.objects.all()
    serializer_class = ArtistSerializer

class SongList(generics.ListCreateAPIView):
    queryset = Song.objects.all()
    serializer_class = SongSerializer

class SongDetail(generics.RetrieveUpdateDestroyAPIView):
    queryset = Song.objects.all()
    serializer_class = SongSerializer
```

## Setting up Authentication
In the urls global file:
```python
from django.conf.urls import include
from django.urls import path
from django.contrib import admin
from django.contrib.auth import views as auth_views

urlpatterns = [
    path('admin/', admin.site.urls),
    path('', include('tunr.urls')),
    path('accounts/login', auth_views.login, name = 'login'),
    path('accounts/logout', auth_views.logout, name = 'logout'),
]
```

Create a folder in templates called Registration and add a file called login.html:
```html
{% extends 'tunr/base.html' %}

{% block content %}
<h2>Login</h2>
<form method="post">
  {% csrf_token %}
  {{ form.as_p}}
  <button type="submit">Login</button>
</form>

{% endblock %}
```

In the base.html, add this:
```html
  <nav>
    <a href="/songs">Songs</a>
    <a href="/">Artists</a>
    <div class="user-info">
      {% if user.is_authenticated %}
        Welcome, {{user.username}}
        <a href="{% url 'logout' %}">Log Out</a>
      {% else %}
        <a href="{% url 'login' %}">Log In</a>
      {% endif %}
    </div>
```

In settings.py
```python
LOGIN_REDIRECT_URL = 'artist_list'
```

In views, you can set what requires login:
```python
from django.contrib.auth.decorators import login_required

@login_required #above any view that requires log in
```