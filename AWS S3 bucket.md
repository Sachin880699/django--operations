Github : <a href="https://github.com/sawardekar/Django_Management">Click Here</a>


# ________________ Create User _______________

# 1) Go to AWS

# 2) Click on Services

# 3) click on ( Security , Identity , & Compliance )

# 4) click on ( IAM )

# 5) Click on ( users )

# 6) click on ( Add User )

# 7) add username and click on ( access key - programmatic access )

# 8) Create Group , group name and search ( AmazonS#FullAccess ) and click on create

# 9) Click on next and again next now click on create user

# 10) now download csv file



# ____________ Now Create S3 Bucket __________________


# 1) go to service click on s3 bucket and click on create button

# 2) Bucket Name , Now uncheck ( Block all public access ) , and click on i acknowledge that and click on create button


# __________________ Now do the settings in django _______________

# 1) In django install ( pip install boto3 )

# 2) install ( pip install django-storages )

# 3) create new file in project filter ( config.py , storage_backends.py )

# 4) end of the settings.py file 


      from .config import *
      AWS_ACCESS_KEY_ID = AMAZON_CREDENTIAL['ACCESS_KEY_ID']
      AWS_SECRET_ACCESS_KEY = AMAZON_CREDENTIAL['SECRET_ACCESS_KEY']
      AWS_STORAGE_BUCKET_NAME = AMAZON_CREDENTIAL['STORAGE_BUCKET_NAME']
      AWS_S3_CUSTOM_DOMAIN = '%s.s3.amazonaws.com' % AWS_STORAGE_BUCKET_NAME
      AWS_S3_OBJECT_PARAMETERS = {
          'CacheControl': 'max-age=86400',
      }
      MEDIAFILES_LOCATION = 'media'
      DEFAULT_FILE_STORAGE = 'Project_Name.storage_backends.MediaStorage'
      
# 5) config.py file

      AMAZON_CREDENTIAL = {
          'ACCESS_KEY_ID': 'AKIART7UPYM6GJTRYQZA',
          'SECRET_ACCESS_KEY': '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',
          'STORAGE_BUCKET_NAME': 'sachinpawar-media'
      }

# 6) storage_backends.py

      from django.conf import settings
      from storages.backends.s3boto3 import S3Boto3Storage

      class MediaStorage(S3Boto3Storage):
          location = settings.MEDIAFILES_LOCATION
          file_overwrite = False
