# Developer Testing Bootcamp

by Melvin Perez-Cedano
Construx Software Training

* We test to reduce the gap between the time when defects are intorduced into the code and the time when those defects are detected

## Adopt a Tester's Mindset

* We need a good definition of testing
* BDD - behavior driven development. Gerkin: [Given When Then](https://martinfowler.com/bliki/GivenWhenThen.html)

## Test Early; Test Often

## Automate your Tests

* use a framework
* don't build tests that test your business logic via the UI
* UI is always changing so we should have less of these tests

## Grow Code with Tests

* TDD

## Integrate Often

* CI

## Ensure Functional Coverage

* Design-by-Contract
* Decision Tables

## Ensure Domain Coverage

* Equivalence Partitioning
* Boundary-Value Analysis
* Pairwise Testing

## Ensure Code Coverage

* Statement Coverage
* Branch Coverage
* Basis Path Coverage
* Multiple Condition Coverage
* MCDC Coverage

## Design for Testability

* Simplify
* Make Dependencies Replaceable
* Test with Doubles

* dependency lookup for higher level tests (integration tests)
    * check a config file

## Write Test Code with Care

* Keep your Test Code Clean
* Test your tests
