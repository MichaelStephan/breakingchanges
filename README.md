# How to deal with breaking changes


## Definition
In YaaS a modification to a service which causes additional work on the customer-side is considered a breaking change. Technically this includes:

* deletion of API endpoint (e.g. adding a new API endpoint is NO breaking change)
* modification of existing service policy, interface or functionality (e.g. adding a new interface or functionality to a service is NO breaking change)

Change examples:

* policy: e.g. use of different authorization mechanism, oauth replaced by SAML 
* interface: e.g. field renamed, shipping-price becomes shipping-costs
* functionaliy: e.g. logic changes, 1+1=2 becomes 1+1=0


## Causes
The authors distinguish between two reasons why breaking changes can occur:

* accidentally (introduction of bug)
* intentionally (change of existing features)


## Avoiding accential breaking changes

An accidental change of the existing interface contract is a normal bug and, like other software bugs, can be prevented by thorough testing.

The applicable test types are:

* functional unit and acceptance tests: verify if functionality is correct. Tests of a former service version can be used against the most recent service version to guarantee that former functional expecations still hold true. In case existing need to be modified special care needs to be taken as it may indicate a change in behaviour 

* [consumer-driven contract tests](http://www.martinfowler.com/articles/consumerDrivenContracts.html): althouh your team provides a service it may rely on some other service. In case the dependent service changes its interface or functionality your service may be broken without your team realizing until hit by a bug tickets raised by customers. A contract test formalizes an expectation towards a downstream service which can be periodically verified. Within the YaaS inner developer circle contract tests can easily be shared and a service provider team can execute its customers' tests during the continuous integration process


## Avoiding and dealing with intentional breaking changes

If your team decides to introduce a new breaking change for an existing API there is a reason. The authors allege that the reasons for breaking changes are mostly:

* bugs,
* immature technical design, and
* immature functional design.

### Bugs
In general serious measures need to be taken to hinder bugs from entering any production environment in the first place; still, it will happen and those bugs need to be fixed quickly. The authors recommend to fix bugs, although by introducing breaking changes, as fast as possible. Ideally a bug is identified in the early stage when only some customers are using a feature. This means that the customer base is still manageable and a team can get in direct contact to support the customer during the change process.

### Immature technical design

* Information hiding: what could be seen in YaaS so far is that the concept of information hiding was not always adhered to. Very often internal details of a service are leaking through its interface, making it espcially hard to change afterwards without negatively impacting the customer. Always ask youself if certain information needs to be available through your service's interface or not.

* Schema strictness: don't design too strict schemas
  * keep as many fields as possible optional
  * don't constrain fields to certain patterns if not really required 

### Immature functional design


## Relieving the customer

* check if there are customers
* contact customer
* support customer

* extended deprecation periods and operate versions in parallel
* communication to consumers of the api
* migration guidelines for consumers
* never lose data 

## Others thoughts

* If a customer changes the behaviour or interface of an existing service with his extension the breaking change was caused by him and he has to deal with the implications.


## Conclusion
Breaking API changes cannot be avoided, they are part of the service lifecycle. What all developers need to do is to minimize the migration efforts for the customers. 

