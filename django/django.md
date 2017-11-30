# Django Cheatsheet

---

## Admin

* Starting dev server
```console
$ python manage.py runserver
```


* Create app
```console
$ python manage.py startapp polls
```

* Can create custom admin site

---

## Models

[models.Model reference](https://docs.djangoproject.com/en/1.11/ref/models/instances/)

**Contains business logic**.

> [A model is the single, definitive source of information about your data. It contains the essential fields and behaviors of the data you’re storing.](https://docs.djangoproject.com/en/1.11/topics/db/models/)

* Subclass of [```django.db.models.Model```](https://docs.djangoproject.com/en/1.11/ref/models/instances/#django.db.models.Model)

### Relationships

#### [Many-to-one](https://docs.djangoproject.com/en/1.11/topics/db/models/#many-to-one-relationships)

[Example](https://docs.djangoproject.com/en/1.11/topics/db/examples/many_to_one/)

* uses [```django.db.models.ForeignKey```](https://docs.djangoproject.com/en/1.11/ref/models/fields/#django.db.models.ForeignKey)
* [ForeignKey docs](https://docs.djangoproject.com/en/1.11/ref/models/fields/#foreign-key-arguments)
* [Following relationships backwards](https://docs.djangoproject.com/en/1.11/topics/db/queries/#backwards-related-objects)

#### Many-to-many relationships

* Uses [```ManyToManyField```](https://docs.djangoproject.com/en/1.11/ref/models/fields/#django.db.models.ManyToManyField)
* Doesn't matter which model you put it in
* If we need to have this relationship hold extra information (Players and Teams is many-to-many, but we want to know when players were on a team), we can create another Model to hold the details on the relationship ([docs](https://docs.djangoproject.com/en/1.11/topics/db/models/#extra-fields-on-many-to-many-relationships))
    * Use ```through``` argument to associate

#### One-to-one relationships

* Uses [```OneToOneField```](https://docs.djangoproject.com/en/1.11/ref/models/fields/#django.db.models.OneToOneField)
* Useful on the primary key of an object when one object "extends" another object
    * Have a Place model (Phone nubmer, address, etc) and a Resturant model. Restaurant is a Place (extends Place), so we can make a 1-1 relationship

### Meta options

> Model metadata is “anything that’s not a field”, such as ordering options (ordering), database table name (db_table), or human-readable singular and plural names (verbose_name and verbose_name_plural). None are required, and adding class Meta to a model is completely optional.

[List of possible Meta options](https://docs.djangoproject.com/en/1.11/ref/models/options/)

### Model Attributes

**Manager** is interface thru which queries are performed. If no custom Manager, default name is objects

### Model Methods




---

## Migrations

[docs](https://docs.djangoproject.com/en/1.11/topics/migrations/)

Steps for making model changes:

1. Change models (```models.py```)

2. Create migrations
```console
$ python manage.py makemigrations polls
```

3.  Run migrations
```console
$ python manage.py migrate
``` 

* Can have [Data Migrations](https://simpleisbetterthancomplex.com/tutorial/2017/09/26/how-to-create-django-data-migrations.html)

---

## Querying

* [Making queries](https://docs.djangoproject.com/en/1.11/topics/db/queries/)
* [Field Lookups](https://docs.djangoproject.com/en/1.11/topics/db/queries/#field-lookups-intro)
* [Related objects](https://docs.djangoproject.com/en/1.11/ref/models/relations/) aka field mappings

* [Query expressions](https://docs.djangoproject.com/en/1.11/ref/models/expressions/#avoiding-race-conditions-using-f)

### Creating Objects

Create a object. ```object.save()```

### Saving Changes to Objects

Change object attribute. ```object.save()```

### Retrieving Objects

* Construction a ```QuerySet```, collection of objects from your database
* Get a query set by using your model's Manager (```.objects()```)

#### Manager methods

* ```.all()```
* ```.filter()```
* ```.exclude()```
* ```.get()``` *returns one object*

* [Field lookups](https://docs.djangoproject.com/en/1.11/topics/db/queries/#field-lookups) aka ```WHERE``` clause

#### ```QuerySet```

* [list of methods](https://docs.djangoproject.com/en/1.11/ref/models/querysets/#queryset-api)
* Can change together filters
* Lazy evaluation

#### Queries across many relationships

* ```Model.objects.filter(relatedmodel__field__contains='Value')``` to get all ```Model``` objects that have a ```field``` which contains ```Value``` in the related model

#### Following relationships 'backwards'

* If a model has a ```ForeignKey```, instances of the foreign-key model will have access to a ```Manager``` that returns all instances of the first model. By default, this ```Manager``` is named ```FOO_set```, where ```FOO``` is the source model name, lowercased.

##### Many-to-many relationship

* The model that defines the ```ManyToManyField``` uses the attribute name of that field itself, whereas the “reverse” model uses the lowercased model name of the original model, plus ```_set``` (just like reverse one-to-many relationships).

* 


---

## Views

* [```django.urls``` docs](https://docs.djangoproject.com/en/1.11/ref/urlresolvers/#module-django.urls)
    * [dynamic urls in templates](https://docs.djangoproject.com/en/1.11/intro/tutorial03/#removing-hardcoded-urls-in-templates)

* [namespacing URL names](https://docs.djangoproject.com/en/1.11/intro/tutorial03/#namespacing-url-names)

* [templates docs](https://docs.djangoproject.com/en/1.11/topics/templates/)

### Generic Views

Lots of our views represent basic Web dev: getting data from the db according to the parameter that was passed, loading a template, and returning rendered template

Shortcut: **generic views system**

To convert:
1. Convert the URLconf
1. Delete old, unneeded views
1. Add views based on Django's generic views


* [Generic display views](https://docs.djangoproject.com/en/1.11/ref/class-based-views/generic-display/#django.views.generic.list.ListView)
* [Class-based views](https://docs.djangoproject.com/en/1.11/topics/class-based-views/)

### Function-Based and Class-Based Views

* 

---

## Testing

* [Tutorial 5](https://docs.djangoproject.com/en/1.11/intro/tutorial05/)
* [Django test Client docs](https://docs.djangoproject.com/en/1.11/topics/testing/tools/#django.test.Client)

* Django test Client returns a ```response.context``` which we can use to make sure that the information we get back is correct

### Good Testing Rules of Thumb

* separate TestClass for each model or view
* separate test method for each set of conditions you want to test
* test method names that describe their function

### Other Kinds of Test

We described backend tests, but we might want to use Selenium to test the way HTML is rendered in the browser.

Look into [```LiveServerTestCase```](https://docs.djangoproject.com/en/1.11/topics/testing/tools/#django.test.LiveServerTestCase)

### Additional Resources

* [Testing in Django](https://docs.djangoproject.com/en/1.11/topics/testing/)
* [Integration with coverage.py](https://docs.djangoproject.com/en/1.11/topics/testing/advanced/#topics-testing-code-coverage)


---

## Static Files

* Throw static files into ```/app/static/app/``` directory

* [Managing Static Files](https://docs.djangoproject.com/en/1.11/howto/static-files/)
* [Deploying Static Files](https://docs.djangoproject.com/en/1.11/howto/static-files/deployment/)


---

## Reuseable App

Can install [app as a package](https://docs.djangoproject.com/en/1.11/intro/reusable-apps/)

> A Django application is just a Python package that is specifically intended for use in a Django project. An application may use common Django conventions, such as having models, tests, urls, and views submodules.



















