# Django-VueJS-App
``` 
Learn from:
https://zephyrnet.com/how-to-prototype-a-web-app-with-django-and-vue-js/
```
## 0. Quick start
After clone this repo, please install virtual environment in it.
```
* virtualenv venv
* source venv/bin/activate
```
Install packges in requirements.txt by this command:
```
pip install -r requirements.txt
```
Ok. Let jump to do Step 15 and 16 to see the final result.
If you want to do it from start, let go from Step 1.

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
#myproject/myapp/models.py
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
#myproject/myapp/admin.py
from django.contrib import admin 
from .models import Article
from .models import Author 
admin.site.register(Article)
admin.site.register(Author)
```

## 6. Write view function to redirect
```
#myproject/myapp/views.py
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

## 8. Create vuejs template for artical-list, author-list, artical-detail, auther-detail
```
#myproject/myapp/templates/myapp/template.html
    <!-- Vue templates -->
    <template id="article-list-template">
      <div class="article-list">
        <h2>Articles</h2>
        <article-item
          v-for="article in articles" 
          v-bind:key="article.slug" 
          v-bind:name="article.name" 
          v-bind:slug="article.slug" 
          v-bind:content="article.content" 
        ></article-item>
      </div>
    </template>
    <template id="author-list-template">
      <div class="author-list">
        <h2>Authors</h2>
        <author-item 
          v-for="author in authors" 
          v-bind:key="author.slug" 
          v-bind:name="author.name" 
          v-bind:slug="author.slug" 
        ></author-item>
      </div>
    </template>
    <template id="article-item-template">
      <div class="article-item">
        <span v-if="$route.params.slug">
          <h3>
            <router-link 
              v-bind:to="'/article/' + $route.params.slug + '/'" v-html="$store.getters.getArticleBySlug($route.params.slug)['name']"
            ></router-link>
          </h3>
          <div v-html="$store.getters.getArticleBySlug($route.params.slug)['content']"></div>
        </span>
        <span v-else>
          <h3>
            <router-link 
              v-bind:to="'/article/' + slug + '/'" 
              v-html="name"
            ></router-link>
          </h3>
          <div v-html="content"></div>
          <hr />
        </span>
      </div>
    </template>
    <template id="author-item-template">
      <div class="author-item">
        <span v-if="$route.params.slug">
          <b>
            <router-link 
              v-bind:to="'/author/' + $route.params.slug + '/'"> 
                [[ $store.getters.getAuthorBySlug($route.params.slug)['name'] ]]
            </router-link>
          </b>
          ([[ $route.params.slug ]]) 
        </span>
        <span v-else>
          <b>
            <router-link 
              v-bind:to="'/author/' + slug + '/'"> 
                [[ name ]]
            </router-link>
          </b>
          ([[ slug ]]) 
        </span>
      </div>
    </template> 
```

## 9. Create components ArticleList, AuthorList, ArticleItem, AuthorItem use above
```
<!-- Vue app -->
  <script>
      // components ArticleList, AuthorList, ArticleItem, AuthorItem
      ArticleList = Vue.component('article-list', {
        data: function () { return { articles: store.state.articles } },
        template: '#article-list-template',
      });

      AuthorList = Vue.component('author-list', {
        data: function () { return { authors: store.state.authors } },
        template: '#author-list-template',
      });

      ArticleItem = Vue.component('article-item', {
        delimiters: ['[[', ']]'],
        props: ['name', 'slug', 'content'],
        template: '#article-item-template',
      });

      AuthorItem = Vue.component('author-item', {
        delimiters: ['[[', ']]'],
        props: ['name', 'slug'],
        template: '#author-item-template',
      });
  </script>
```

## 10. Create Vuex Store in Django template
```
<script>
#myproject/myapp/templates/myapp/template.html
  // Create Vuex Store in Django template
  const store = new Vuex.Store({ 
    state: { 
      authors: [ 
        {% for author in authors %} 
          { name: '{{ author.name }}', slug: '{{ author.slug }}', }, 
        {% endfor %} ], 
      articles: [ 
        {% for article in articles %} 
          { content: '{{ article.content | linebreaksbr }}', name: '{{ article.name }}', slug: '{{ article.slug }}', }, 
        {% endfor %} ], 
      }, 
      getters: { 
        getArticleBySlug: (state) => (slug) => { 
          return state.articles.find(articles => articles.slug === slug) }, 
        getAuthorBySlug: (state) => (slug) => { 
          return state.authors.find(authors => authors.slug === slug) }, 
      }
  })
</script>
```

## 11. Update URL
```
#myproject/myproject/urls.py
from django.contrib import admin
from django.urls import path 
# don't forget to import the app's view!
from myapp import views as myapp_views 
urlpatterns = [ 
    path('admin/', admin.site.urls), # paths for our app
    path('', myapp_views.frontend),
    path('article/', myapp_views.frontend),
    path('author/', myapp_views.frontend),
    path('article/<slug:slug>/', myapp_views.frontend), 
    path('author/<slug:slug>/', myapp_views.frontend),
]
```

## 12. Set vue router
```
#myproject/myapp/templates/myapp/template.html
<scrip>
      //router
      // Set Vue Route
      const routes = [ 
          { component: ArticleList, path: '/article/', }, 
          { component: AuthorList, path: '/author/', }, 
          { component: ArticleItem, path: '/article/:slug/', }, 
          { component: AuthorItem, path: '/author/:slug/', },
      ] 
      const router = new VueRouter({ mode: 'history', routes: routes,})
</scrip>
```

## 13. Register vue app
```
#myproject/myapp/templates/myapp/template.html
<script>
  // app
  // Register Vue app
  const myapp = new Vue({
    router,
    store,
  }).$mount('#myapp');
</script>
```
## 14. Register app to setting
* Register myapp to myproject
```
#myproject/myproject/settings.py
INSTALLED_APPS = [
    ........
    'myapp.apps.MyappConfig',
]
```
* I also change DATABASES because I use mySQL, but you can ignore if you don't want to use
```
#myproject/myproject/settings.py
DATABASES = {  
    'default': {  
        'ENGINE': 'django.db.backends.mysql',  
        'NAME': 'DjangoVueJSApp',  
        'USER': 'root',  
        'PASSWORD': 'password',  
        'HOST': '127.0.0.1',  
        'PORT': '3306',  
        'OPTIONS': {  
            'init_command': "SET sql_mode='STRICT_TRANS_TABLES'"  
        }  
    }  
}
```

## 15. Create Django supper user
* python manage.py createsuperuser

## 16. Run app
Use the command
* python manage.py runserver
```And then go to http://127.0.0.1:8000/admin/. After login, let create at least 2 authors and 2 artical for each author
```
Finally, go to http://127.0.0.1:8000 and see the final result

