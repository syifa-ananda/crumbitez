
**Questions and Answers**
1. Explain how you implemented the checklist above step-by-step (not just following the tutorial).

- Create a new directory with the name ```crumbitez```
- Open the terminal and create a virtual environment by running the command 
```
python3 -m venv env
```
- Activate the virtual environment with the command
```
source env/bin/activate
```
- Inside the same directory, create a new file named ```requirements.txt``` and fill it with some dependencies
```
django
gunicorn
whitenoise
psycopg2-binary
requests
urllib3
```
- Install the dependencies with the command
```
pip install -r requirements.txt
```
- Create a Django project named ```crumbitez``` with the command
```
django-admin startproject crumbitez .
```
- In the settings.py, add the following line of code to the ALLOWED_HOSTS
```
...
ALLOWED_HOSTS = ["localhost", "127.0.0.1"]
...
```
- Create an application called main with command
```
python manage.py startapp main
```
- After the app main installed, added main to the INSTALLED_APPS in the file settings.py
```
INSTALLED_APPS = [
    ...,
    'main'
]
```
- Create a new directory named templates inside the main application. Inside the templates directory, create a new file named main.html and fill it with
```
<h1>{{ app_name }} </h1>
<h5>Name: </h5>
<p>{{ name }}</p>
<h5>Class: </h5>
<p>{{ class }}</p>
```
- Open the models.py file in the main application directory and fill it with this code
```
from django.db import models

class Product(models.Model):
    name = models.CharField(max_length=100)
    price = models.IntegerField()
    description = models.TextField()
    quantity = models.IntegerField()
```
- Open the views.py file located in the main application file and fill it with
```
def show_main(request):
    context = {
        'app_name': 'crumbitez',
        'name': 'Syifa Ananda Widyati',
        'class': 'PBD KKI'
    }

    return render(request, 'main.html', context)
```
- Open the ```urls.py``` file in the main directory and fill it with the code
```
from django.urls import path
from main.views import show_main

app_name = 'main'

urlpatterns = [
    path('', show_main, name='show_main'),
]
```
- Open the urls.py file inside of the crumbitez project and fill it with code
```
from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('', include('main.urls')),
]
```
Git and PWS deployment
- Made a new repository in github and connect it with my local repository
- After connected, do a add, commit, and push
- For deployment, create a PWS project named crumbitez. Then add ```syifa-ananda.31.pbp.cs.ui.ac.id``` to ALLOWED_HOSTS in setting.py
```
ALLOWED_HOSTS = ["localhost", "127.0.0.1", "syifa-ananda31-crumbitez.pbp.cs.ui.ac.id"]
```
- Push to PWS repository for the deployment

2. Create a diagram that contains the request client to a Django-based web application and the response it gives, and explain the relationship between urls.py, views.py, models.py, and the html file.
![alt text](diagram2.png)

3. Explain the use of git in software development!

Git is a tool in software development that helps manage code changes and enables collaboration among developers on project versions and features or fixes branches before integrating them into the main codebase repository effectively maintaining a record of alterations, for easy rollback when necessary. 

4. In your opinion, out of all the frameworks available, why is Django used as the starting point for learning software development?

Django is often chosen as a starting point for learning software development because it's a high-level, beginner-friendly web framework that encourages clean, organized code and follows best practices like the DRY (Don't Repeat Yourself) principle. It comes with built-in features like authentication, database management, and an admin panel, allowing learners to focus on creating applications rather than starting form scratch each time. Its strong documentation and active community support also make it an ideal choice for beginners.

5. Why is the Django model called an ORM?

Django models are called an ORM (Object-Relational Mapping) because they allow developers to work with databases using Python objects than directly writing SQL queries form scratch. The ORM automatically translates Python code (like creating, reading, updating, or deleting data) into SQL commands that are executed on the database. This abstraction simplifies database operations and ensures that developers work with database records as if they were Python objects, making development more intuitive.