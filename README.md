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
## 7. Write template.html as the main page
```
#myproject/myapp/templates/myapp/template.html
<!doctype html>
<html lang="en">
  <head>
    <!-- Required meta tags --> 
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no">
    <!-- Bootstrap CSS --> 
    <link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.4.1/css/bootstrap.min.css" integrity="sha384-Vkoo8x4CGsO3+Hhxv8T/Q5PaXtkKtu6ug5TOeNV6gBiFeWPGFN9MuhOf23Q9Ifjh" crossorigin="anonymous">
    <script src="https://unpkg.com/vue"></script> 
    <script src="https://unpkg.com/vue-router"></script> 
    <!-- jQuery first, then Popper.js, then Bootstrap JS --> 
    <script src="https://code.jquery.com/jquery-3.4.1.slim.min.js" integrity="sha384-J6qa4849blE2+poT4WnyKhv5vZF5SrPo0iEjwBvKU7imGFAV0wwj1yYfoRSJoZ+n" crossorigin="anonymous"></script> 
    <script src="https://cdn.jsdelivr.net/npm/popper.js@1.16.0/dist/umd/popper.min.js" integrity="sha384-Q6E9RHvbIyZFJoft+2mJbHaEWldlvI9IOYy5n3zV9zzTtmI3UksdQRVvoxMfooAo" crossorigin="anonymous"></script> 
    <script src="https://stackpath.bootstrapcdn.com/bootstrap/4.4.1/js/bootstrap.min.js" integrity="sha384-wfSDF2E50Y2D1uUdj0O3uMBJnjuUD4Ih7YwaYd1iqfktj0Uod8GCExl3Og8ifwB6" crossorigin="anonymous"></script>
    <title>Django and Vue.js</title>
    <style>
      .router-link-active {
        color: black;
        text-decoration: none;
      }
    </style>
  </head>
  <body class="bg-light">
    <div class="bg-white container">
      <h1>Prototyping a Web App with Django and Vue.js</h1>
      <!-- Content -->
      
    </div>
    </div>
    <!-- Vue.js -->
    <script src="https://cdn.jsdelivr.net/npm/vue@2.6.12/dist/vue.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/vue-router/2.6.0/vue-router.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/vuex/2.5.0/vuex.js"></script>
    <!-- Vue templates -->
    
    <!-- Vue app -->
    
  </body>
</html>

```
 python manage.py createsuperuser

pip freeze > requirements.txt