# what is user of the redis 
ANS : The reson is if we use redis we does not need to execute query every time when any user hit request that time data will store in cache 
     memeory but when another user hit query that time will receve that user data from cache memory not from database memory
     
     
 
 # Step to add Radis 
 
 
 
 # Step 1
 
 1 ) sudo apt-get install redis-server
 
 2 ) wget https://download.redis.io/releases/redis-4.0.6.tar.gz 
 
 3 ) tar xzf redis-4.0.6.tar.gz 
 
 4 ) cd redis-4.0.6
 
 5 ) make
 
 5 ) sudo redis-server
 
 
 # Step - 2
 
 
 1 )  create virtual env and activate that
 
 2 )  install library
 
      pip install django==1.9
      pip install django-redis
      pip install djangorestframework
      
3 ) django-admin startproject django_cache

4 ) cd django_cache

5 ) python manage.py startapp store

6 ) Settings.py 
      # settings.py
      INSTALLED_APPS = [
          'django.contrib.admin',
          'django.contrib.auth',
          'django.contrib.contenttypes',
          'django.contrib.sessions',
          'django.contrib.messages',
          'django.contrib.staticfiles',
          'store', # add here
          'rest_framework', # add here too
      ]
7 ) store/models.py

      from __future__ import unicode_literals
      from django.db import models
      import datetime

      # Create your models here.


      class Product(models.Model):

          name = models.CharField(max_length=255)
          description = models.TextField(null=True, blank=True)
          price = models.IntegerField(null=True, blank=True)
          date_created = models.DateTimeField(auto_now_add=True, blank=True)
          date_modified = models.DateTimeField(auto_now=True, blank=True)

          def __unicode__(self):
              return self.name

          def to_json(self):
              return {
                  'id': self.id,
                  'name': self.name,
                  'desc': self.description,
                  'price': self.price,
                  'date_created': self.date_created,
                  'date_modified': self.date_modified
              }
              
8 ) python manage.py makemigration store

9 ) python manage.py migrate

10 ) python manage.py createsuperuser

11 ) settings.py

      CACHES = {
          'default': {
              'BACKEND': 'django_redis.cache.RedisCache',
              'LOCATION': 'redis://127.0.0.1:6379/',
              'OPTIONS': {
                  'CLIENT_CLASS': 'django_redis.client.DefaultClient',
              }
          }
      }
12 ) store/views.py

      from rest_framework.decorators import api_view
      from rest_framework import status
      from rest_framework.response import Response
      from django.core.cache import cache
      from django.conf import settings
      from django.core.cache.backends.base import DEFAULT_TIMEOUT

      CACHE_TTL = getattr(settings, 'CACHE_TTL', DEFAULT_TIMEOUT)
      from .models import Product

      # Create your views here.



      @api_view(['GET'])
      def view_books(request):

          products = Product.objects.all()
          results = [product.to_json() for product in products]
          return Response(results, status=status.HTTP_201_CREATED)


      @api_view(['GET'])
      def view_cached_books(request):
          if 'product' in cache:
              # get results from cache
              products = cache.get('product')
              return Response(products, status=status.HTTP_201_CREATED)

          else:
              products = Product.objects.all()
              results = [product.to_json() for product in products]
              # store data in cache
              cache.set(products, results, timeout=CACHE_TTL)
              return Response(results, status=status.HTTP_201_CREATED)
              
13 ) store/urls.py

      from django.urls import include, path
      from.views import *

      urlpatterns = [
          path("",view_books),
          path("cache/",view_cached_books)
      ]
      
 14 ) django_cache/urls.py
 
 
      from django.contrib import admin
      from django.urls import path , include

      urlpatterns = [
          path('admin/', admin.site.urls),
          path("store/",include("store.urls"))
      ]


15 ) sudo npm install -g loadtest

      loadtest -n 100 -k  http://localhost:8000/store/
      
16 ) store/views.py


      from rest_framework.decorators import api_view
      from rest_framework import status
      from rest_framework.response import Response
      from django.core.cache import cache
      from django.conf import settings
      from django.core.cache.backends.base import DEFAULT_TIMEOUT

      CACHE_TTL = getattr(settings, 'CACHE_TTL', DEFAULT_TIMEOUT)
      from .models import Product

      # Create your views here.



      @api_view(['GET'])
      def view_books(request):

          products = Product.objects.all()
          results = [product.to_json() for product in products]
          return Response(results, status=status.HTTP_201_CREATED)


      @api_view(['GET'])
      def view_cached_books(request):
          if 'product' in cache:
              # get results from cache
              products = cache.get('product')
              return Response(products, status=status.HTTP_201_CREATED)

          else:
              products = Product.objects.all()
              results = [product.to_json() for product in products]
              # store data in cache
              cache.set(products, results, timeout=CACHE_TTL)
              return Response(results, status=status.HTTP_201_CREATED)
              
17 ) store/urls.py

      from django.urls import include, path
      from.views import *

      urlpatterns = [
          path("",view_books),
          path("cache/",view_cached_books)
      ]
      
      
18 ) loadtest -n 100 -k  http://localhost:8000/store/cache/
 
