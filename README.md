# How to deal with breaking changes

## Definition
In YaaS a modification to a service which causes additional work on the customer-side is considered a breaking change. Technically this includes:

* deletion of API endpoint (e.g. adding a new API endpoint is NO breaking change)
* modification of existing service interface or functionality (e.g. adding a new interface or functionality to a service is NO breaking change)

## Causes
The authors distinguish between two reasons why breaking changes can occur:

* accidentally (introduction of bug)
* intentionally (change of existing features)

## Avoiding accential breaking changes

* extensive testing (contract testing)

## Avoiding and dealing with intentional breaking changes

* better planning
* better API design, no internals

## Relieving the customer

* extended deprecation periods and operate versions in parallel
* communication to consumers of the api
* migration guidelines for consumers

## Others thoughts

* If a customer changes the behaviour or interface of an existing service with his extension the breaking change was caused by him and he has to deal with the implications.

## Conclusion
Breaking API changes cannot be avoided, they are part of the service lifecycle. What all developers need to do is to minimize the migration efforts for the customers. 

