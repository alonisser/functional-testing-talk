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

* Red green refactor
 
---

# Why tests?

* Validate our code is actually doing what we expect.
* Promotes good (better..) design, clearer api, decoupling.
* Faster development cycles (Really!) catching bugs early and fast.
* Ability to refactor (even a year later when the code does not look familiar anymore)
* Ours users are not our QA "volunteers" .


---
# Naming issues

functional vs Integration vs system vs .. tests
* Looks like everyone "knows" (or has an opinion) what is a "unit" but the rest is a complete mess
* **MY** definition are tests using the app public http api, "beneath" the UI.
* We can also use Martin Fowler "subcutaneoustest" if we can pronounce it.

---

# The traditional view: The Test Pyramid

![](https://martinfowler.com/bliki/images/testPyramid/test-pyramid.png)

---

# The traditional view: The Test Pyramid

* "Unit is fast" use unit
* "DB is hot lava" avoid at all costs
* Higher level tests are slow and brittle, don't do that, at least not too much.
---

# The traditional view: The Test Pyramid
An often missed side note:
"
The pyramid is based on the assumption that broad-stack tests are expensive, slow, and brittle compared to more focused tests, 
such as unit tests. While this is usually true, there are exceptions. If my high level tests are fast, reliable, 
and cheap to modify - then lower-level tests aren't needed.

"

---

# functional testing: the new unit?

* In an Interesting twitter "war" not long ago, DHH (of Rails fame) claimed that the "db is part of your app now" and db hitting tests are the new unit.
 
---

# Functional testing

* What do we test
 
---

# Functional testing patterns

* Naming
 
---

# Functional testing patterns

* verify zero state
 
---

# Functional testing patterns

* Terse and readable tests.
* Separate technical test code

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
 
---

# Functional testing patterns: Replacing third party collaborators

---

# Functional testing patterns: Globals

Globals are also collaborators that you might want to control

* Time

* Logging

(See GOOS for examples)

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

* 

---

# Testing taxonomy 

* http://martinfowler.com/bliki/subcutaneoustest.html
* http://martinfowler.com/bliki/BroadStackTest.html
---

# interesting resources and further reading

* GOOS book
* http://www.obeythetestinggoat.com/fast-tests-useless-hot-lava-be-damned.html
* https://www.youtube.com/watch?v=raxiirphs9k
* http://martinfowler.com/bliki/GivenWhenThen.html
* http://martinfowler.com/bliki/TestPyramid.html



---

class: center, middle

# open source rocks!
## thanks for listening!

[live presentation](https://alonisser.github.io/functional-testing-talk) <br/>
[twitter](alonisser@twitter.com), [medium](https://medium.com/@alonisser/), 
[sad frontend il](http://sadfrontendil.tumblr.com/)

---
