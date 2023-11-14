# Django-VueJS-App
``` 
Learn from:
https://zephyrnet.com/how-to-prototype-a-web-app-with-django-and-vue-js/
```

## 1. Install env
* virtualenv venv
* source venv/bin/activate       # Active the venv
* pip install --upgrade pip
* python -m pip install Django   # Install Django

## 2. Create python project name myproject, python app myappp
* django-admin startproject myproject #Create project name myproject
* cd myproject
* django-admin startapp myapp # Create app name my app
* pip install mysqlclient
```
The folder structure
  Django-VueJS-App
  |- myproject
    |- myapp
    |- myproject
    |- manage.py
  |- venv
  |- README.md
  |- requirements.txt
```
## 3. Create 2 models Artical and Author in myapp/models.py

```
**myapp/models.py**
from django.db import models
class Article(models.Model): 
  """Table schema to store articles.""" 
  name = models.CharField(max_length=64) 
  author = models.ForeignKey('myapp.Author', on_delete=models.CASCADE) 
  content = models.TextField() 
  slug = models.CharField(default='', max_length=64) 
  def __str__(self): 
    return '%s' % self.name 
  
class Author(models.Model): 
  """Table schema to store auhtors.""" 
  name = models.CharField(max_length=64) 
  slug = models.CharField(default='', max_length=64) 
  def __str__(self): 
    return '%s' % self.name
```

## 4. Run to migration 
* python manage.py makemigrations ***# Create new migration based on the change to models***
* python manage.py migrate ***# Apply migrations***

## 5. Register to use those above models in admin site
```
# myapp/admin.py
from django.contrib import admin 
from .models import Article
from .models import Author 
admin.site.register(Article)
admin.site.register(Author)
```

## 6. Write view function to redirect
```
#myapp/views.py
from django.shortcuts import render
from .models import Article
from .models import Author
def frontend(request): 
  """Vue.js will take care of everything else.""" 
  articles = Article.objects.all() 
  authors = Author.objects.all() 
  data = { 'articles': articles, 'authors': authors, } 
  return render(request, 'myapp/template.html', data)
```

 python manage.py createsuperuser

pip freeze > requirements.txt