class: center, middle

# functional testing

##Or: "public app api driven tests"

###why and how?

[live presentation](https://alonisser.github.io/functional-testing-talk) <br/>

[twitter](alonisser@twitter.com), [medium](https://medium.com/@alonisser/), 
[sad frontend il](http://sadfrontendil.tumblr.com/)
#####Recommanded! also read my political blog: [דגל אדום](degeladom@wordpress.com)
---

# TDD in one sentence 

* Test Driven Development
* Please note: This does not say: always test first.

---

# TDD in one sentence

* Red green refactor
 
---

# Why tests?

* Validate our code is actually doing what we expect.
* Promotes good (better..) design, clearer api, decoupling.
* Faster development cycles (Really!) catching bugs early and fast.
* Ability to refactor (even a year later when the code does not look familiar anymore)
* Ours users are not our QA "volunteers" .


---
# The traditional view: The Test Pyramid

* "Unit is fast" use unit
* "DB is hot lava" avoid at all costs
* Higher level tests are slow and brittle, don't do that, at least not too much.
---


# The traditional view: The Test Pyramid

![](https://martinfowler.com/bliki/images/testPyramid/test-pyramid.png)

---

# Naming issues

functional vs Integration vs system vs .. tests
* Looks like everyone "knows" (or has an opinion) what is a "unit" but the rest is a complete mess
* **MY** definition are tests using the app public http api, "beneath" the UI.
* Tests cover a **single** app, not inter app integration (That would be under **End To End** or **Integration** testing)
* We can also use Martin Fowler "subcutaneoustest" if we can pronounce it.

---
# Naming issues

Even more confusion 
* UI Testing isn't the same thing as End to End testing
* UI Testing isn't the same as Higher level testing. components encapsulating logic can and should be tested using unit testing.

---
# The traditional view revisited: Problems With UI driven testing

Selenium and similar tools:
* SLOW! 
* BRITTLE. Broken easily on UI changes (Even with best practice patterns)
* FLAKY. 5-10% failure due to "network" or "browser" conditions renders the tests worthless. 

---
# The traditional view revisited: Problems With Unit testing

* The trees and forest problem.
* Too much mocking is dangerous, we can break the class but the tests still pass
* Might lead to tight coupling of tests to implementation.
* Blamed for design monstrosities, needed to abstract all db and Io code out of tested classes. 

---

# The traditional view revisited: The Test Pyramid
An often missed side note:

"
    The pyramid is based on the assumption that broad-stack tests are expensive, slow, and brittle compared to more focused tests, 
    such as unit tests. While this is usually true, there are exceptions. If my high level tests are fast, reliable, 
    and cheap to modify - then lower-level tests aren't needed.

"

---

# The traditional view revisited: functional testing - the new unit?

* In an Interesting twitter "war" not long ago, @DHH (of Rails fame) claimed that the "db is part of your app now" and db hitting tests are the new unit.
 
---

# Functional testing: High level overview

* Testing features! not technical items.
* Testing the public Api of a specific app. Not testing internals
* Driving the tests through the app api, no mocking, replacing Third party collaborators. 
---

# Functional testing patterns

* Meaningful Naming
 
---

# Functional testing patterns

* verify zero state
 
---

# Functional testing patterns

* Terse and readable tests.
* Separate technical test code

---
# Functional testing patterns

* BDD style methods (http://martinfowler.com/bliki/GivenWhenThen.html)

 
---

# Functional testing patters: Setup and data creation

* The problem with fixtures

---

# Functional testing patterns: Setup and data creation

* data builders
 
---
class: center, middle

# Anti Patterns

* An Intermezzo
 
---
# CI: Does A test exists if it does not run?

* "Sure we have automatic tests"
* Wiring up a Continues Integration process is a build in part of writing tests. 
* If you don't have one, **You don't really have tests**
 
---


# The commented out test anti pattern

* "Sure we have automatic tests"
* OH
 
---

# Functional testing patterns: Replacing third party collaborators with fake collaborators
* Wrapping third party collaborators in proxies. 
* Proxies should not used directly but "injected" into our code
Instead of this:

```python
class SMSDistributionHandler(BaseDistributionHandler):
    
    def __init__(self):
        self.sms_sender = SMSSendingService()

```

Do this:
```python
class SMSDistributionHandler(BaseDistributionHandler):
    
    def __init__(self):
        self.sms_sender = ServiceLocator.get_service('sms_sender')
```

* Similar to the way Django allows us to replace some backends with fake backends in settings.py

---

# Functional testing patterns: Asserting against fake collaborators
```python

class TestApp(object):

    def verify_sms_has_not_been_sent(self, message, receiver_phone, sender_phone):
        sms_messages = self._get_sms_by_predicate(lambda x: x.message == message and x.sender == sender_phone and
                                                            x.recipient == receiver_phone)
        self.assertFalse(len(sms_messages))

    def _get_sms_by_predicate(self, predicate):
        return query(self.sms_service.messages).where(predicate).to_list()

```

---

# Functional testing patterns: Globals

Globals are also collaborators that you might want to control

* Time

* Logging

(See GOOS for more examples)

---
# Functional testing patterns: Handling global time

* No magical or implicit datetime handling in models (no auto_add_now)
* use a programmable wrapper over ```datatime.now()```

```python
# -*- coding: utf-8 -*
from django.utils import timezone

time_settings = {
    'frozen_time': False

}

def freeze_time(frozen_time):
    time_settings['frozen_time'] = frozen_time

def unfreeze_time():
    time_settings['frozen_time'] = None

def now():
    if time_settings['frozen_time']:
        return time_settings['frozen_time']
    return timezone.now()

```
```python
import clock
clock.now()
```
---

# When do I unit? 

* Certainly not advocating against unit testing. sometimes unit testing is more valuable/thorough and or quicker.
For example:
    * A component encapsulating logic: a rule engine or a calculator class for example.
    * A UI component with inner logic mapped to presentation: React element, django template tag
    * Simple helpers: A formatting helper for example. 

Generally speaking: Pure functional code encapsulated within a class/function, without IO, is a good candidate for unit testing
---

# When do I unit?

* THIS presentation is about apps, web apps. If you are writing a library then Unit testing is probably more suited for your needs. 

---

# What you should take from this talk

* Test you app. Please. 
* Treat testing as an integral part of coding your app.
* You'll thank me for this

---

# Testing taxonomy 

* http://martinfowler.com/bliki/subcutaneoustest.html
* http://martinfowler.com/bliki/BroadStackTest.html
---

# interesting resources and further reading

* [GOOSG book](http://www.growing-object-oriented-software.com/)
* http://www.obeythetestinggoat.com/fast-tests-useless-hot-lava-be-damned.html
* https://www.youtube.com/watch?v=raxiirphs9k
* http://martinfowler.com/bliki/GivenWhenThen.html
* http://martinfowler.com/bliki/TestPyramid.html
* [TDD is Dead. long live testing/@DHH](http://david.heinemeierhansson.com/2014/tdd-is-dead-long-live-testing.html)
* [Is TDD Dead? conversions between Kent Beck, David Heinemeier Hansson, and Martin fowler](https://martinfowler.com/articles/is-tdd-dead/)



---

class: center, middle

# open source rocks!
## thanks for listening!

[live presentation](https://alonisser.github.io/functional-testing-talk) <br/>
[twitter](alonisser@twitter.com), [medium](https://medium.com/@alonisser/), 
[sad frontend il](http://sadfrontendil.tumblr.com/)

---
