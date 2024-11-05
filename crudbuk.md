
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


# PART 2

# CRUD Operations
To demonstrate CRUD (Create, Read, Update, Delete) operations and cover model relationships in Django, let's extend the existing `blog` app with a simple model representing blog posts. We'll then implement views and templates to perform CRUD operations on these blog posts.


### Model Section:
The model section defines the structure of your data. In Django, models are Python classes that represent database tables. Each model class defines fields that correspond to table columns.

**Step 1: Define the Model**

Create a model named `Post` in the `models.py` file within the `blog` app directory. This model will represent individual blog posts and will have a title, content, author, and publication date.

```python
from django.db import models
from django.contrib.auth.models import User

class Post(models.Model):
    title = models.CharField(max_length=200)
    content = models.TextField()
    author = models.ForeignKey(User, on_delete=models.CASCADE)
    publication_date = models.DateTimeField(auto_now_add=True)

    def __str__(self):
        return self.title
```
**Explanation:**
- We define a `Post` model with fields `title`, `content`, `author`, and `publication_date`.
- The `title` field is a character field with a maximum length of 200 characters.
- The `content` field is a text field to store longer content.
- The `author` field is a foreign key linking to the `User` model from Django's built-in authentication system.
- The `publication_date` field is a date and time field that automatically sets the current date and time when a new post is created.
- The `__str__` method is defined to return the title of the post when the model instance is converted to a string.

**Make Migrations and Migrate**

Run the following commands to create and apply migrations for the new model:

```python
python manage.py makemigrations
python manage.py migrate
```

**Register Model with Admin**

Registering the models with the Django admin site is an important step to enable CRUD operations via the Django admin interface. Let's add the necessary admin configurations for the `Post` model.

Open the `admin.py` file within the `blog` app directory and add the following code:

```python
from django.contrib import admin
from .models import Post

admin.site.register(Post)
```

This code registers the `Post` model with the Django admin site, allowing administrators to manage blog posts through the admin interface.

**Creating a Superuser**

To access the Django admin interface, we need to create a superuser account. Run the following command in your terminal or command prompt:

```python
python manage.py createsuperuser
```
Follow the prompts to enter a username, email (optional), and password for the superuser account.

**Step 2: Run the Server**

Ensure the Django development server is running, and navigate to the admin interface (`http://127.0.0.1:8000/admin`) in your web browser. Log in with the credentials of a superuser, and you should see the `Posts` section, where you can perform CRUD operations on blog posts.


**Step 2: Make Migrations and Migrate**

Run the following commands to create and apply migrations for the new model:

```bash
python manage.py makemigrations
python manage.py migrate
```

**Step 3: CRUD Views**

### Views Section:

#### Explanation:
Views in Django are functions or classes that handle web requests and return web responses. Views contain the business logic of your application and interact with models to fetch or manipulate data.

Create views to handle CRUD operations for the `Post` model. Add the following code to the `views.py` file within the `blog` app directory:

```python
from django.shortcuts import render, get_object_or_404, redirect
from .models import Post
from .forms import PostForm

def post_list(request):
    posts = Post.objects.all()
    return render(request, 'blog/post_list.html', {'posts': posts})

def post_detail(request, pk):
    post = get_object_or_404(Post, pk=pk)
    return render(request, 'blog/post_detail.html', {'post': post})

def post_create(request):
    if request.method == 'POST':
        form = PostForm(request.POST)
        if form.is_valid():
            post = form.save(commit=False)
            post.author = request.user
            post.save()
            return redirect('post_detail', pk=post.pk)
    else:
        form = PostForm()
    return render(request, 'blog/post_form.html', {'form': form})

def post_edit(request, pk):
    post = get_object_or_404(Post, pk=pk)
    if request.method == 'POST':
        form = PostForm(request.POST, instance=post)
        if form.is_valid():
            post = form.save(commit=False)
            post.author = request.user
            post.save()
            return redirect('post_detail', pk=post.pk)
    else:
        form = PostForm(instance=post)
    return render(request, 'blog/post_form.html', {'form': form})

def post_delete(request, pk):
    post = get_object_or_404(Post, pk=pk)
    post.delete()
    return redirect('post_list')
```

#### Example: `post_list` View
```python
from django.shortcuts import render, get_object_or_404, redirect
from .models import Post
from .forms import PostForm

def post_list(request):
    posts = Post.objects.all()
    return render(request, 'blog/post_list.html', {'posts': posts})
```

**Explanation:**
- We define a view named `post_list` that retrieves all posts from the database using the `Post.objects.all()` method.
- The retrieved posts are passed to the `post_list.html` template using the `render` function along with the request object.
- The `post_list.html` template will be responsible for rendering the list of posts in the browser.


#### Example: `post_detail` View

```python
from django.shortcuts import render, get_object_or_404, redirect
from .models import Post
from .forms import PostForm


def post_detail(request, pk):
    post = get_object_or_404(Post, pk=pk)
    return render(request, 'blog/post_detail.html', {'post': post})
```

**Explanation:**
- The `post_detail` view retrieves a specific post based on its primary key (`pk`).
- It uses the `get_object_or_404` shortcut to either retrieve the post with the specified primary key or raise a 404 error if the post does not exist.
- The retrieved post object is then passed to the `post_detail.html` template for rendering.

#### Example: `post_create` View

```python
from django.shortcuts import render, get_object_or_404, redirect
from .models import Post
from .forms import PostForm

def post_create(request):
    if request.method == 'POST':
        form = PostForm(request.POST)
        if form.is_valid():
            post = form.save(commit=False)
            post.author = request.user
            post.save()
            return redirect('post_detail', pk=post.pk)
    else:
        form = PostForm()
    return render(request, 'blog/post_form.html', {'form': form})
```

**Explanation:**
- The `post_create` view handles the creation of a new post.
- If the request method is `POST`, it creates a form instance with the data from the request.
- If the form data is valid, it creates a new `Post` object but doesn't save it to the database yet (`commit=False`).
- It sets the author of the post to the current user (assuming user authentication is implemented).
- Finally, it saves the post to the database and redirects the user to the `post_detail` view for the newly created post.

#### Example: `post_edit` View

```python
from django.shortcuts import render, get_object_or_404, redirect
from .models import Post
from .forms import PostForm

def post_edit(request, pk):
    post = get_object_or_404(Post, pk=pk)
    if request.method == 'POST':
        form = PostForm(request.POST, instance=post)
        if form.is_valid():
            post = form.save(commit=False)
            post.author = request.user
            post.save()
            return redirect('post_detail', pk=post.pk)
    else:
        form = PostForm(instance=post)
    return render(request, 'blog/post_form.html', {'form': form})
```

**Explanation:**
- The `post_edit` view allows users to edit an existing post.
- It retrieves the post object to be edited based on its primary key (`pk`).
- If the request method is `POST`, it creates a form instance populated with the data from the request and the instance of the post being edited.
- If the form data is valid, it updates the post object with the new data and saves it to the database.
- After successful editing, it redirects the user to the `post_detail` view for the edited post.

#### Example: `post_delete` View

```python
from django.shortcuts import render, get_object_or_404, redirect
from .models import Post
from .forms import PostForm

def post_delete(request, pk):
    post = get_object_or_404(Post, pk=pk)
    post.delete()
    return redirect('post_list')
```

**Explanation:**
- The `post_delete` view handles the deletion of a post.
- It retrieves the post object to be deleted based on its primary key (`pk`).
- It then calls the `delete()` method on the post object to delete it from the database.
- After deletion, it redirects the user to the `post_list` view to display the updated list of posts.

**Step 4: Forms**

Create a form to handle input for the `Post` model. Create a file named `forms.py` within the `blog` app directory and add the following code:

```python
from django import forms
from .models import Post

class PostForm(forms.ModelForm):
    class Meta:
        model = Post
        fields = ['title', 'content']
```

**Step 5: URLs**

Update the `urls.py` file within the `blog` app directory to include URLs for CRUD views:

```python
from django.urls import path
from . import views

urlpatterns = [
    path('', views.post_list, name='post_list'),
    path('post/<int:pk>/', views.post_detail, name='post_detail'),
    path('post/new/', views.post_create, name='post_create'),
    path('post/<int:pk>/edit/', views.post_edit, name='post_edit'),
    path('post/<int:pk>/delete/', views.post_delete, name='post_delete'),
]
```

**Step 6: Templates**

Create templates for listing posts, displaying post details, and creating/editing posts. Place these templates within a `blog/templates/blog` directory.

- `post_list.html`:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Post List</title>
</head>
<body>
    <h1>Post List</h1>
    <ul>
        {% for post in posts %}
        <li><a href="{% url 'post_detail' pk=post.pk %}">{{ post.title }}</a></li>
        {% endfor %}
    </ul>
</body>
</html>
```

- `post_detail.html`:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>{{ post.title }}</title>
</head>
<body>
    <h1>{{ post.title }}</h1>
    <p>{{ post.content }}</p>
    <p>Author: {{ post.author }}</p>
    <p>Publication Date: {{ post.publication_date }}</p>
</body>
</html>
```

- `post_form.html`:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>{% if form.instance.pk %}Edit Post{% else %}Create Post{% endif %}</title>
</head>
<body>
    <h1>{% if form.instance.pk %}Edit Post{% else %}Create Post{% endif %}</h1>
    <form method="post">
        {% csrf_token %}
        {{ form.as_p }}
        <button type="submit">Save</button>
    </form>
</body>
</html>
```

**Step 7: Linking Views to URLs**

Ensure that the views are correctly linked to the URLs in the `urls.py` file of the `blog` app.

**Step 8: Run the Server**

Start the Django development server and navigate to the appropriate URLs to test the CRUD functionality for blog posts.
