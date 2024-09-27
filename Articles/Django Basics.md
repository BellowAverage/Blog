# Django Basics

Created: September 12, 2024 1:48 PM
Labels & Language: Django, Python, Shell
Source: https://www.youtube.com/watch?v=sm1mokevMWk&t=201s

## Description

This is a tutorial notes covering the basics of building web applications using the Django web framework. Learn how to create and modify apps, direct browsers to different views with Views module, and manage URLs with the URLs module.

[Django For Beginners - Full Tutorial](https://www.youtube.com/watch?v=sm1mokevMWk&t=201s)

## Django Basics

### Create an application

This creates an app. An app can normally be considered as an independent function, and modification on this particular function can be done within the app’s folder.

```bash
python manage.py startapp <app's name>
```

### Views module

`views.py` contains functions that direct the browser to different views created.

```python
def index(response):
    return HttpResponse("<h1>Hello World</h1>")
```

This, for example, displays “Hellow world” on the page `…/index`. `views.py` is considered as a distributor of many html pages with a Django project.

### Urls module

`urls.py` is considered as register of urls. Here’s an instance.

```python
urlpatterns = [
    path("", views.index, name="index"),
    path("v1/", views.view1, name="view 1")
]
```

### Apps registeration

Go to `settings.py` and install apps created. `app01`, for example, add `app01.apps.App01Config` in `INSTALLED_APPS` list in settings. Then apply change by running the following in terminal.

```bash
python manage.py migrate
```

### Models module

`models.py` is where database operation happens. Class is used to define table in a database. Each model, in my current opinion, is similar to the concept of table in a database, meanwhile, the concept of models, or `models.py` in Django, is like a schema in MySql.

```python
class ToDoList(models.Model):
    name = models.CharField(max_length=200)

    def __str__(self):
        return self.name

class Item(models.Model):
    todolist = models.ForeignKey(ToDoList, on_delete=models.CASCADE)
    text = models.CharField(max_length=300)
    complete = models.BooleanField()
    
    def __str__(self):
        return self.text
```

After defining the database, go to the terminal to apply changes in models. This creates records of changes in the app’s migrations folder, so that changes review is made possible, just like logs or versions system.

```bash
python manage.py makemigrations <name of the app>  # app01 in this instance
python manage.py migrate  # apply everything again
```

### Add data into the database using shell

This is a quick way to modify database. The example shows how to add “Chris’ List” in the table ToDoList.

```bash
(venv) chriswang@chriswang-virtual-machine:~/mysite$ python3 manage.py shell
Python 3.10.6 (main, Nov 14 2022, 16:10:14) [GCC 11.3.0] on linux
Type "help", "copyright", "credits" or "license" for more information.
(InteractiveConsole)
>>> from app01.models import Item, ToDoList
>>> t = ToDoList(name="Chris\' List")
>>> t.save()
>>> ToDoList.objects.all()
<QuerySet [<ToDoList: Chris' List>]>
>>> ToDoList.objects.get(id=1)
<ToDoList: Chris' List>
```

Here’s how to set attributes to a created primary key.

```bash
...
>>> t
<ToDoList: Chris' List>
>>> t.item_set.all()
<QuerySet []>
>>> t.item_set.create(text="Go to the mall", complete=False)
<Item: Go to the mall>
>>> t.item_set.all()
<QuerySet [<Item: Go to the mall>]>
```

### Templates

Use block to create reproductive structures, in whcih customisable contents can be added in the block. Things in there are default value. Change them in its children by simply overriding the original contents.

```html
<div id="content">
    {% block content %}
    {% endblock %}
</div>
```

### models.Model class

**`models.Model`** is a base class in Django's ORM that provides several commonly used functions for creating and manipulating database models. 

- **Here are some commonly used functions and their explanations.**
    - **`objects`**: This is a manager object that allows querying of the database table associated with the model. For example, **`User.objects.all()`** would return all the **`User`** objects in the database.
    - **`save()`**: This method saves the current instance of the model to the database. If the model is new, it will be inserted as a new row; if it already exists in the database, it will be updated.
    - **`delete()`**: This method deletes the current instance of the model from the database.
    - **`get_or_create()`**: This method attempts to retrieve a row from the database that matches the given criteria. If the row doesn't exist, it creates a new instance and saves it to the database.
    - **`create()`**: This method creates a new instance of the model and saves it to the database in a single step.
    - **`update()`**: This method updates one or more fields of one or more rows that match the given criteria.

These are just a few of the commonly used functions provided by **`models.Model`**. The full list of methods and attributes can be found in Django's documentation.

### ORM Basics

- **Instance of models.Model class**

`ls = user.objects.get(user_account="admin").user_id` is how the inquiry is done. Much more convenient than using traditional SQL.

`ls = user.objects.all()[1].user_id` for example, the output is the value under user_id column, 2nd row.

- **Manipulations of dimensions**

```python
# 当表中已经有数据存在的情况下如果添加新的字段，那么需要设置null或者default
password = models.IntegerField(verbose_name='密码',null=True)  #verbose_name=相当于comment，null=True表示可为空
age = models.IntegerField('年龄',default=18)

# 剩下的删改查，修改代码即可，另外补充：
# 1.主键字段orm会自动创建 只不过名字固定为id
# 2.CharField字段类型必须要有max_length参数(CharField等同varchar , max_length(32)表示varchar的字符数)
```

- **Manipulations of values**

```python
# 查
models.User.objects.all()  # 查询user表中所有的数据
    # select * from user;
    <QuerySet [<User: jerry>, <User: tony>, <User: kevin>]>  #返回的是一个Queryset [数据对象]

models.User.objects.filter(name='jerry')  # 查询name=jerry的数据
    # select * from user where name='jerry'
    <QuerySet [<User: jerry>]>  

models.User.objects.filter(name='jerry',password=123)
    # select * from user where name='jerry' and password=123
    <QuerySet [<User: jerry>]>
"""
QuerySet我们可以简单的理解为是列表套一个个数据对象
"""

# 增
models.User.objects.create(name='jerry',password=567)
    # insert into user(name,password) values('jerry',567)
    上述orm返回值为当前被创建出来的数据对象本身

obj = models.User(name='tom',password=111)  # 类产生对象
obj.name ='jerry'
obj.save()  # 对象调用save方法保存到数据库

# 改
edit_obj = models.User.objects.filter(id=3)[0] #因为得到的是一个列表套数据对象，所以需要[0]取值才是数据对象
edit_obj.name = 'kevinSB'
edit_obj.save()

models.User.objects.filter(name='jason').update(name='jasonNB')
    # update user set name='jasonNB' where name='jason';

# 删
models.User.objects.filter(id=5).delete()
    # delete from user where id=5;
```

- **See more about the methods provided by `ORM` here.**

[django框架--orm基本使用](https://zhuanlan.zhihu.com/p/437349848)

### models.Model._meta attribute

In Django, the **`_meta`** attribute provides metadata about the model, including fields, database table name, ordering, and many other attributes. 

- **Here are some commonly used functions of `_meta` .**
    1. **`get_field(field_name)`**: Returns the **`Field`** instance for the given field name.
    2. **`get_fields()`**: Returns a list of **`Field`** instances for all fields in the model.
    3. **`get_all_field_names()`**: Returns a list of all field names in the model.
    4. **`get_field_names()`**: Returns a list of field names for fields that are not related.
    5. **`get_all_related_objects()`**: Returns a list of **`RelatedObject`** instances for all related objects.
    6. **`get_field_by_name(field_name)`**: Returns a tuple consisting of the **`Field`** instance for the given field name and the name of the model field.
    7. **`get_many_to_many_fields()`**: Returns a list of **`ManyToManyField`** instances for all many-to-many relationships in the model.
    8. **`ordering`**: Returns a tuple of fields to order by, as specified in the model's **`Meta`** class.
    9. **`db_table`**: Returns the database table name for the model.
    10. **`verbose_name`**: Returns the human-readable name for the model, as specified in the model's **`Meta`** class.

These are just a few of the functions available in **`_meta`**. The full list can be found in the Django documentation.

### Utilize pymysql to operate database

The `ORM` database manipulation has remarkable advantage, yet some may still be more familiar with `SQL` commands, and feel more comfortable operating in this rather traditional way. To do this, configuring as follow.

```python
import pymysql

# Connect to the MySQL database
connection = pymysql.connect(
    host='localhost',
    user='root',
    password='password',
    db='mydatabase',
    cursorclass=pymysql.cursors.DictCursor
)

# Create a cursor object
cursor = connection.cursor()

# Execute a SELECT statement
cursor.execute("SELECT * FROM user")

# Fetch the first row of data
row = cursor.fetchone()
print(row)

# Fetch all rows of data
rows = cursor.fetchall()
for row in rows:
    print(row)

# Execute an INSERT statement
sql = "INSERT INTO user (user_id, user_account, user_password, user_auth_level) VALUES (%s, %s, %s, %s)"
values = (1, 'john@example.com', 'password123', 1)
cursor.execute(sql, values)
connection.commit()

# Execute an UPDATE statement
sql = "UPDATE user SET user_account = %s WHERE user_id = %s"
values = ('john@example.org', 1)
cursor.execute(sql, values)
connection.commit()

# Execute a DELETE statement
sql = "DELETE FROM user WHERE user_id = %s"
values = (1,)
cursor.execute(sql, values)
connection.commit()

# Close the cursor and connection
cursor.close()
connection.close()
```

### Utilize python in html under Django

Utilizing Python in HTML templates under Django involves using template tags, which are Python functions that can be used in HTML templates to dynamically render content based on the data passed to the template.

Here are some examples of using template tags in Django templates:

- **Looping over a list**

```html
<ul>
{% for item in items %}
  <li>{{ item }}</li>
{% endfor %}
</ul>
```

This will iterate over the **`items`** list and render each item in an HTML **`<li>`** tag.

- **Conditionally rendering content**

```html
{% if user.is_authenticated %}
  <p>Welcome, {{ user.username }}!</p>
{% else %}
  <p>Please log in to access this page.</p>
{% endif %}
```

This will check if the user is authenticated and render a personalized welcome message if they are, or a message prompting them to log in if they are not.

- **Using variables in HTML**

```html
<p>{{ page_title }}</p>
```

This will render the value of the **`page_title`** variable in an HTML **`<p>`** tag.

- **Calling Python functions**

```html
{% with my_list|reverse as reversed_list %}
  <ul>
    {% for item in reversed_list %}
      <li>{{ item }}</li>
    {% endfor %}
  </ul>
{% endwith %}
```

This will call the **`reverse`** function on the **`my_list`** variable and assign the result to a new variable called **`reversed_list`**. It will then iterate over the reversed list and render each item in an HTML **`<li>`** tag.