# Mock Notes

## Michael Foord (creator of mock library)

### [Part 1](https://www.youtube.com/watch?v=d3_LdLzWSXQ)

* Mock is a library to create mock (fake) objects that can be used to replace parts of your system under test.
* Mock is a spy, fake, double object
* Mock follows Arrange, Act, Assert pattern of testing
	* Arrange - set up system under test
	* Act - call the system under test
	* Assert - make sure the right things have happened

* ```from unittest.mock import:```
	* MagicMock (like Mock, but supports ```__magic__``` methods)
	* patch
	* call
	* ANY

* Mock object are mocks all the way down
	* access attribute, returns mock object
	* call a function, returns a mock object

* Mock objects track their usage (```.called```, ```.call_count```, ```.mock_calls```)
	* pass a mock object into our system under test
	* system uses it in a certain way
	* we make assertions about how it was used

* PyCon 2016 mock talk by ana something
	* cause of truthy value, make sure your asserts are right
	* but we can use spec=True, to make sure we aren't magically passing tests

### [Part 2](https://www.youtube.com/watch?v=ADIcwu2GeEU)

* Why use mock?
	* Isolation
	* Determinism (deterministic models produce the same output from a given startinc condition or initial state)	* make test reliable, producing the same answer every time
	* Speed / Performance
	* Avoiding dependencies on external systems

* Unit tests are isolated, need to be fast, and need useful tracebacks

* Use mocks objects to fake out parts of an external API to make tests deterministic and compare to known preprocessed results

### [Part 3](https://www.youtube.com/watch?v=yFA-FFaEZPo)

* Django test runner starts up a test database. All this time adds up
* Use mocks to mock out things like the database
* patch lets you replace objects in your system with mock objects
	* monkeypatching
		* easy to do, don't forget to replace
		* context manager is a good way because it has a closing step
			* and you control scope of patch
		* function decorator
		* class decorator, applies same patch to every single method in the class
			* but does it at creation time, decorator is not reapplied
* Step to do mock
	* figure out how the object will be use
	* mock that behavior
	* use the mock object as it should be
	* assert it was used the way you intended

* What to assert
	* response.status_code
	* assertIn('text', response.content)
	* use [pyquery](https://pythonhosted.org/pyquery/), jquery-esque lib from python, to test against actual stuff

* patch is interesting
	* do the patch where the name is looked up
	* [timestamp](https://youtu.be/yFA-FFaEZPo?t=16m)

#### Question

* Should we test render? It's a django function after all
	* BUT YES we should because templates have logic
	* If you have branches for different templates, make sure you test those too

## [Stop Mocking, Start Testing](https://www.youtube.com/watch?v=Xu5EhKVZdV8)

* dependency injection does not mean this

```python
def my_func(database=None):
  if database is None:
    # construction
```

## [The Tao of Testing](http://jasonpolites.github.io/tao-of-testing/index-1.1.html)

* talking about how to use dependency injection to get things done 

---

## To Read / Watch

## Fake It Til You Make It: Unit Testing Patterns With Mocks and Fakes

[YouTube link](https://www.youtube.com/watch?v=hvPYuqzTPIk&t=20s)

