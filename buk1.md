
# PART 1

### Introduction to Django

Django is a high-level Python web framework that encourages rapid development and clean, pragmatic design. It follows the model-template-view (MTV) architectural pattern, which emphasizes the separation of concerns and allows developers to build complex web applications quickly and efficiently.

**Step 1: Setting up the environment and creating a virtual environment**

First, navigate to the directory where you want to create your project. Then, create a virtual environment. Virtual environments help isolate your Python environment and dependencies for each project, which is considered a best practice. In your terminal or command prompt, run:

```bash
python -m venv venv
```

This command will create a virtual environment named myblog_env in your current directory.

### Activating the virtual environment

Activate the virtual environment by running the appropriate script for your operating system. In Windows, run:

```bash
venv\Scripts\activate
```

And in Unix or MacOS, run:

```bash
source venv/bin/activate
```

You should see (venv) at the beginning of your command prompt, indicating that the virtual environment is activated.

**Step 2: Installing Django**

Django is a high-level Python web framework that simplifies web development by providing various tools and libraries for building web applications. In this step, we're installing Django using pip, which is Python's package manager.

```bash
pip install django
```

**Step 3: Creating a Django project**

A Django project is a collection of settings, configurations, and apps that work together to create a web application. We use the `django-admin` command-line utility to create a new Django project.

```bash
django-admin startproject myblog_project
```

**Step 4: Creating a Django app**

Django apps are modular components that encapsulate specific functionality within a project. Each app typically serves a distinct purpose, such as managing user authentication, handling blog posts, or serving static files. We create a new app named `blog` using the `startapp` command.

```bash
python manage.py startapp blog
```

**Step 5: Create `urls.py` for the app**

Create a file named `urls.py` inside the `blog` directory:

```bash
touch blog/urls.py
```

**Contents of blog/urls.py:**

```python
from django.urls import path
from . import views

urlpatterns = [
    path('', views.home, name='home'),
]
```

**Explanation:** We define URL patterns specific to the `blog` app in the `urls.py` file. Here, we have a single URL pattern that maps the root URL (`''`) to the `home` view function defined in `views.py`.

This approach allows the `blog` app to manage its own URL patterns independently.

**Step 6: Update project-level `urls.py`**

Open `urls.py` inside the `myblog_project` directory, and include the URLs of the blog app:

```python
from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('', include('blog.urls')),
]
```

**Explanation:** We include the URL patterns defined in the `blog/urls.py` file into the project's URL patterns using Django's `include` function.

**Project and App Files:**

```
myblog_project/
├── blog/
│   ├── migrations/
│   ├── templates/
│   │   └── home/
│   │       └── home.html
│   ├── __init__.py
│   ├── admin.py
│   ├── apps.py
│   ├── models.py
│   ├── tests.py
│   ├── urls.py   # <-- New file
│   ├── views.py
├── myblog_project/


│   ├── __init__.py
│   ├── settings.py
│   ├── urls.py
│   └── wsgi.py
├── manage.py
└── myblog_env/
```

With these changes, the `blog` app now has its own `urls.py` file, providing better organization and modularity. The project-level `urls.py` includes the URL patterns from the `blog` app, allowing the app's URLs to be accessed within the project.

**Step 7: Creating a template folder**

In Django, templates are used to generate HTML dynamically. Templates allow us to separate the presentation layer (HTML) from the business logic of our application. We create a folder named `templates` within our `blog` app directory to store HTML templates.

```bash
mkdir blog/templates
mkdir blog/templates/home
```

**Step 8: Creating the HTML template**

We create an HTML template file named `home.html` within the `templates/home` folder. This template will be used to render the home page of our blog application.

1. ```bash
touch blog/templates/home/home.html
    ```
2. Open `home.html` and add the following content:

   ```html
   <!DOCTYPE html>
   <html lang="en">
   <head>
       <meta charset="UTF-8">
       <meta name="viewport" content="width=device-width, initial-scale=1.0">
       <title>Hello Beginner</title>
   </head>
   <body>
       <h1>Hello beginner</h1>
   </body>
   </html>
   ```

**Step 9: Defining views**

Views in Django are Python functions or classes that receive web requests and return web responses. Views contain the logic for processing requests and rendering responses. We define a view named `home` within the `views.py` file of our `blog` app to handle requests to the home page.

```python
from django.shortcuts import render

def home(request):
    return render(request, 'home/home.html')
```

**Step 10: Defining URLs**

URL patterns in Django map URLs to views. Each URL pattern is defined using a Python regular expression (regex) and specifies which view should handle requests to that URL. We create a `urls.py` file within the `blog` app directory to define URL patterns for our application.

```python
from django.urls import path
from . import views

urlpatterns = [
    path('', views.home, name='home'),
]
```

**Step 11: Including the blog URLs in the project**

We include the URL patterns of our `blog` app in the project's main URL configuration file (`urls.py`). This allows requests to be routed to the appropriate views within the `blog` app.

```python
from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('', include('blog.urls')),
]
```

**Step 12: Running the development server**

Finally, we start the Django development server to test our application locally. The development server serves our Django application over HTTP, allowing us to access it through a web browser and test its functionality.

```bash
python manage.py runserver
```

Once the development server is running, we can navigate to `http://127.0.0.1:8000/` in a web browser to view our Django application. If everything is set up correctly, we should see the message "Hello beginner" rendered on the home page.

By following these steps, we've created a simple Django project with a home page that displays a message using an HTML template. This setup demonstrates the basic structure of a Django project, including virtual environment setup, app creation, template creation, view definition, URL configuration, and running the development server.


