---
 title: Unit Testing 1 -- Unit tests, Integration tests and Test-driven development
 date: {date}
 categories: Unit Testing
---

## What is unit test?

* A _unit test_ is a piece of a code (usually a method) that invokes another piece of code and checks the correctness of some assumptions afterward.
* If the assumptions turn out to be wrong, the unit test has failed. 
* A “unit” is a method or function.

## Properties of a good unit test

* It should be __automated__ and __repeatable__.
* It should be __easy to implement__.
* Once written, it should remain for __future use__.
* Anyone should be able to run it.
* It should run at the push of a button(__Good tests should be easily executed in their original form, not manually__).
* It should run quickly.
* __Ask the following questions before write any unit test cases__:
    * Can I run and get results from a unit test I wrote two weeks or months or years ago?
    * Can any member of my team run and get the results from unit tests I wrote two months ago?
    * Can I run all the unit tests I’ve written in no more than a few minutes?
    * Can I run all the unit tests I’ve written at the push of a button?
    * Can I write a basic unit test in no more than a few minutes?

<!-- More -->

## What is Integration tests

_Integration testing_ means testing two or more dependent software modules as a group.

## Drawbacks of integration tests

* You could not run and get result from the test you wrote someting later. In this case, __you don't know whether you broke a existing feature__.
* People are afraid of changing the existing code because when they add __new features, they could not run the existing integration tests case and get the expected result__.
* It takes time to run integration tests, people will __run integration tests less often__, which result in __getting feedbacks later__ and __spend more time to find bugs__ if there is any.
* It may requires some configuration or input to run the integration tests and __people may avoid to run them__.
* It takes time to write integration tests, __people may not likely to write them__ and __many parts of the core logic in the code may not be tested__.

## Test-driven development

* _TDD_ is test-first development, which means write tests before writing production codes.
* Process of TDD:
    1. Write Tests
    2. Run all tests.
    3. Write code to make tests pass.
    4. Refactor codes and fix bugs.
    5. Repeat the above steps until all tests pass.

* Benefits
    * Quality code
    * Quality tests
    * Better design of codes, reducing complexity.
    * Make sure the code coverage of your test code.
* Drawbacks
    * time to learn and implement


