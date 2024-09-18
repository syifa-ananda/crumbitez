## Assignment 2

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


## Assignment 3

**Question and Answers**
1. Explain why we need data delivery in implementing a platform.

Data delivery is essential for implementing a platform as it enables real-time access to information, facilitating prompt decision-making and seamless platform operations. It enables scalability as data volumes increase and facilitates smooth integration of different data sources, such as APIs or IoT devices, improving the overall user experience. Reliable data delivery reduces delays and ensuring consistent performance. This is crucial for platforms handling high traffic or needing real-time processing.

2. In your opinion, which is better, XML or JSON? Why is JSON more popular than XML?

In my opinion, JSON is better than XML for most modern applications due to its simplicity, lightweight structure, and native support in JavaScript, making it easier to use and more efficient for data transmission. JSON's popularity is driven by its readability, smaller file sizes, and quicker parsing in web development, whereas XML is more verbose and complex, making it less suitable for contemporary web-based systems.
 
3. Explain the functional usage of is_valid() method in Django forms. Also explain why we need the method in forms.

In Django forms, is_valid() method is used to validate the data submitted in a form according to the validations defined by the form. When the form is triggered, it prepares every field in the form to be validated against whatever criteria you have (like required fields, type of fields and custom validators). True means the data is valid in which case form.SetValue method validated returns true and gives us the form with validated data. cleaned_data. Otherwise it returns False and you can access the errors in form.errors. This method is important to ensure that only valid and clean data is processed or saved, which helps to maintain data integrity and prevent errors in application logic.

4. Why do we need csrf_token when creating a form in Django? What could happen if we did not use csrf_token on a Django form? How could this be leveraged by an attacker?

The csrf_token in Django forms is used to protect against Cross-Site Request Forgery (CSRF) attacks, in which a malicious request is submitted without the user's awareness due to trickery by an attacker. This token guarantees that the form was submitted from the same site and not externally. If the csrf_token is not used, a fake form that submitted requests on behalf of the user might be placed on some other site which could enable unauthorized actions such as changing account details, transferring funds or deleting data. Without the CSRF protection, these types of attacks could compromise the security and integrity of the web application.
 
5. Explain how you implemented the checklist above step-by-step

**Create a form input to add a model object to the previous app**

- Create a new file in the main directory with the name ```forms.py``` to create the structure of the form and fill it with
```
from django.forms import ModelForm
from main.models import Product

class CrumbitezEntryForm(ModelForm):
    class Meta:
        model = Product
        fields = ["name", "price", "description", "quantity"]
```
- Open the ```views.py``` file in the main directory. Add ```from django.shortcuts import render, redirect``` and create the function with the name ```create_product```
```
def create_product(request):
    form = CrumbitezEntryForm(request.POST or None)

    if form.is_valid() and request.method == "POST":
        form.save()
        return redirect('main:show_main')

    context = {'form': form}
    return render(request, "create_product.html", context)
```
- Change the ```show_main``` function that already exists in the ```views.py``` file to the following
```
def show_main(request):
    crumbitez_entries = Product.objects.all()

    context = {
        'app_name': 'crumbitez',
        'name': 'Syifa Ananda Widyati',
        'class': 'PBD KKI',
        'product_entries': 'crumbitez_entries',
    }

    return render(request, 'main.html', context)
```
- Open the ```urls.py``` file and import the ```create_product``` function and add the URL path to the ```urlpatterns``` varaible
```
from main.views import show_main, create_product
```
```
urlpatterns = [
   ...
   path('create-product', create_product, name='create_product'),
]
```
- Create new HTML file named  ```create_product.html``` in the ```main/templates``` and add the following code
```
{% extends 'base.html' %} 
{% block content %}
<h1>Add New Product</h1>

<form method="POST">
  {% csrf_token %}
  <table>
    {{ form.as_table }}
    <tr>
      <td></td>
      <td>
        <input type="submit" value="Add Product" />
      </td>
    </tr>
  </table>
</form>

{% endblock %}
```

**Add 4 views to view the added objects in XML, JSON, XML by ID, and JSON by ID formats**

- Open the ```views.py``` file and add the ```HttpResponse``` and ```Serializer``` imports at the top of the file
```
from django.http import HttpResponse
from django.core import serializers
```
- Create four new function in the ```views.py```
```
def show_xml(request):
    data = Product.objects.all()
    return HttpResponse(serializers.serialize("xml", data), content_type="application/xml")

def show_json(request):
    data = Product.objects.all()
    return HttpResponse(serializers.serialize("json", data), content_type="application/json")

def show_xml_by_id(request, id):
    data = Product.objects.filter(pk=id)
    return HttpResponse(serializers.serialize("xml", data), content_type="application/xml")

def show_json_by_id(request, id):
    data = Product.objects.filter(pk=id)
    return HttpResponse(serializers.serialize("json", data), content_type="application/json")
```

**Create URL routing for each of the views**

- Open the ```urls.py``` file and import the function that we just created
```
from main.views import show_main, create_product, show_xml, show_json, show_xml_by_id, show_json_by_id
```
- Add the URL path to the ```urlpatterns``` variable in the ```urls.py``` file to access the function that was imported in the previous point
```
...
path('xml/', show_xml, name='show_xml'),
path('json/', show_json, name='show_json'),
path('xml/<str:id>/', show_xml_by_id, name='show_xml_by_id'),
path('json/<str:id>/', show_json_by_id, name='show_json_by_id'),
```

**Postman Result**

- XML
![alt text](xml.png)

- XML by ID
![alt text](xmlId.png)

- JSON
![alt text](json.png)

- JSON by ID
![alt text](jsonId.png)
