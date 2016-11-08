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

Functional requirements will always chanage and so will functionality exposed through your service. Keep the functionality exposed to the other world to a minimum. Only provide what customers are really asking for. Make sure that the functionality offered by your service is implemented in a generic way so it can be extended if really required. Prior realeasing verify with various stakeholders that the functionality is inline with expectations. 


## Relieving the customer

* Always check if there customers out there really using the functionality you are going to change. Always get in touch with them prior any changes (direct, mail, etc.) 
* If there is only a limited set of customers impacted it may help to support them during the migration process more actively 
* Extend the deprecation periods of your serivces and run multipe versions of your service in parallel. There are two ways how to do so:
  * Merge the new/ old functionality into one service and serve from a single runtime
  * Run one runtime per version
* Provide your customers with detail guides on what changes and how to migrate from one version to another
* Always make sure that data being available in one version is also available in the next version. This must not require the customer to take action 


## What a customer can do to relieve himself

Alhtough the introduction of a breaking changes requires a customer to migrate to the most recent version he could have taken precautions to keep the efforts to a minimum:

* Be relaxed when doing schema checks, only check the fields you are really using 
* Be relaxed in your de-marshalling (JSON -> language specific DTO) processes, don't fail if there are unknown/ missing fields 
* Make sure to make your logic independent from other service's DTO objects  
* etc.

## Others thoughts

* If a customer changes the behaviour or interface of an existing service with his extension the breaking change was caused by him and he has to deal with the implications.


## Conclusion
Breaking API changes cannot be avoided, they are part of the service lifecycle. What all developers need to do is to minimize the migration efforts for the customers. 


# Appendix

## A breaking change

In case of any of the given events a change is considered as breaking change:

* renaming a field
* removing a field
* changing mime types
* changing response codes
* changing resource names
* changing query parameters
* changing headers
* changing the type of a field
* changing existing business functionality
* changing the url of a service
* changing the authentication/ authorization scheme
* changing a scope required to access a certain resource
* etc.

The rules apply to all sorts of interfaces, REST interfaces but also pub/sub messages. 
