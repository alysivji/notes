# Django Testing

## Testing Basics

### Running tests

Have files named test_*.py

```console
$ python manage.py test
```

### Test Database

* Tests are not done on production databases, but on a blank database created for tests
* This database is destroyed once all tests have been executed

### Test Order Execution

1. All TestCase subclasses are run first
1. All other Django-based tests are run
1. unitteset.TestCase tests are run

### Misc

* Any initial data loaded in migrations will only be available in TestCase tests and not in TransactionTestCase tests
* tests are run with ```DEBUG=False``` obviously ;)
* [running tests in parallel](https://docs.djangoproject.com/en/1.11/ref/django-admin/#cmdoption-test-parallel)

---

## Django Test Client

* check if correct template is being rendered
* verify the correct context is being passed in
* examine status codes and url

* does not need Web server to be running
    * avoids overhead of HTTP and deals with Django framework directly

```python
from django.test import Client

c = Client()
response = c.get('/endpoint/',
                 data={'foo': 2, 'bar': 3})
```

* test client is stateful, stores cookies are res

### Testing Responses

* test response object has additional information that is useful for test code to verify

* client
* content - body of response
* context - context instance that was used to render template
* json - body of response, parsed as JSON
* request
* status_code
* templates - templates used to generate response
* resolver_match - use this to find which view served the response

### Authentication

* test login using ```django.test.Client.login```
    * After you call this method, the test client will have all the cookies and session data required to pass any login-based tests that may form part of a view.
    * After logging in, can test views that are only available to logged-in users
    * Done in test database so we will also need to create accounts

### Miscellaneous

* exceptions that are not visible to test client are Http404, PermissionDenied, SystemExit, and Suspicious Operation
    * Django converts these to HTTP response codes

* test client is stateful, reponse returns a cookie which will be stored in client and sent with all subsequent ```get()``` and ```post()``` reponses
    * ```Client.cookies``` and ```Client.session```

### Django Test Case Classes

* [SimpleTestCase](https://docs.djangoproject.com/en/1.11/topics/testing/tools/#simpletestcase)
    * Use this for all Django Tests
* [TransactionTestCase](https://docs.djangoproject.com/en/1.11/topics/testing/tools/#transactiontestcase) **Test specific database transaction behavior**
* [TestCase](https://docs.djangoproject.com/en/1.11/topics/testing/tools/#testcase) **Most common way to write test**
    * Use these for an y tests that make a database query
* [LiveServerTestCase](https://docs.djangoproject.com/en/1.11/topics/testing/tools/#django.test.LiveServerTestCase)
    * Use this for Selenium client. Test rendered HTML and behavior of webpages
    * Used for functional testing inside browser and to simulate a real user's actions

> SimpleTestCase and its subclasses (e.g. TestCase, …) rely on setUpClass() and tearDownClass() to perform some class-wide initialization (e.g. overriding settings). If you need to override those methods, don’t forget to call the super implementation

---

## Test Case Features

https://docs.djangoproject.com/en/1.11/topics/testing/tools/#test-cases-features

https://docs.djangoproject.com/en/1.11/topics/testing/advanced/
