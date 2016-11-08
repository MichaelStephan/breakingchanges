# How to deal with breaking changes

## Assumptions
The authors created this document as a summary of common best practices already engrained with YaaS teams. It should encourage team members to think about the topic of breaking changes in order to avoid unnecessary mistakes.


## Definition
In YaaS a modification to a service causing additional work on the customer's side is considered a breaking change. Technically this includes:

* deletion of API endpoint (e.g. adding a new API endpoint is NO breaking change)
* modification of existing service policy, interface or functionality (e.g. adding a new interface or functionality to a service is NO breaking change)

Change examples:

* policy: e.g. replacement of different authorization mechanism, oauth replaced by SAML 
* interface: e.g. field renamed, shipping-price becomes shipping-costs
* functionality: e.g. logic changes, 1+1=2 becomes 1+1=0

Please refer to the appendix for a more detailed definition. 


## Causes
The authors distinguish between two reasons why breaking changes can occur:

* accidentally (introduction of bug)
* intentionally (change of existing behaviour)


## Avoiding accidental breaking changes
An accidental change of the existing interface contract is a normal bug and, like other software bugs, can be prevented by thorough testing.

The applicable test types are:

* functional unit and acceptance tests: verify if functionality is correct. Tests of a former service version can be used against the most recent service version to guarantee that former functional expectations still hold true. In case existing tests need to be modified special care needs to be taken as it may indicate a change in behaviour. This process is referred to as common regression testing 

* [consumer-driven contract tests](http://www.martinfowler.com/articles/consumerDrivenContracts.html): although your team provides a service it may rely on some other service. In case the dependent service changes its interface or functionality your service may break without your team realizing until hit by bug tickets raised by customers. A contract test formalizes an expectation towards a downstream service which can be periodically verified. Within the YaaS inner developer circle contract tests can be easily shared and a service provider team can execute its customers' tests during the continuous integration process. In case a new build would break others expectations it is guaranteed that the functionality never makes it into the production system


## Avoiding and dealing with intentional breaking changes
If your team decides to introduce a new breaking change for an existing API there is a reason. The authors allege that the reasons for breaking changes are mostly:

* bugs,
* immature technical design, and
* immature functional design.

### Bugs
In general serious measures need to be taken to hinder bugs from entering any production environment in the first place; still, it will happen and those bugs need to be fixed quickly. The authors recommend to fix bugs, although by introducing breaking changes, as fast as possible. Ideally a bug is identified in the early stage when only some customers are using a feature. This means that the customer base is still manageable and a team can get in direct contact to support the customer during the change process. Take measures to un-publish the buggy version as soon as possible.

The given advice applies to various sort of bugs:

* security
* functional
* etc.

### Immature technical design
A violation of the following principles will also lead to breaking changes: 

* Information hiding: what could be seen in YaaS so far is that the concept of information hiding was not always adhered to. Very often internal details of a service are leaking through its interface, making it especially hard to change afterwards without negatively impacting the customer. Always ask yourself if certain functionality/ information needs to be available through your service's interface or not.

* Schema strictness: don't design too strict and rigid schema definitions
  * keep as many fields as possible optional
  * don't constrain fields to certain patterns if not really required (within your service you may still do checks to be in compliance with certain security standards) 

### Immature functional design
Functional requirements will always change and so will functionality exposed through your service. Keep the functionality exposed to the outer world to a minimum. Only provide what customers really ask for. Make sure that the functionality offered by your service is implemented in a generic way so it can be extended if required. Prior to releasing verify with various stakeholders that the functionality is in-line with their expectations to be able to make adaptations before it is too late.


## Relieving the customer

* Always get in touch with your customers prior to any changes (direct, mail, etc.) 
* If there is only a limited set of customers impacted it may help to support them during the migration process more actively 
* Extend the deprecation periods of your services and run multiple versions of your service in parallel. There are two ways how to do so:
  * Merge the new/ old functionality into one service and serve from a single runtime
  * Run one runtime per version
* Provide your customers with detail reports on what changes occurred and offer guidelines on how to migrate from one version to another
* Always make sure that data available in one version is also available in the next version. This MUST NOT require the customer to take action


## What a customer can do to help himself
Although the introduction of a breaking changes requires a customer to migrate to the most recent version he could have taken precautions to keep the implementation efforts to a minimum:

* Be relaxed when doing schema checks, only check the fields you are really using 
* Be relaxed in your de-marshalling (JSON -> language specific DTO) processes, don't fail if there are unknown/ missing fields 
* Make sure to make your logic independent from other service's DTO objects  
* Implement contract tests assuring your remote service's behaviour
* Don't expose an alien interface via your service
* etc.


## Conclusion
Breaking API changes cannot be avoided, they are part of the service lifecycle. The best weapon to fight against breaking changes are detailed functional and technical planning and industry best practice testing procedures. When breaking changes are still required it is the developers task to minimize the migration efforts for the customers. 


# Appendix

## A breaking change definition
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

Non-functional characteristics of a service may also lead to breaking changes:

* SLA figures change (decrease in availability, maximum response times are extended)
* etc.

The same applies to commercial or contractual changes; both cause breaking changes:

* contracts are changed
* prices are changed


## Other thoughts

* If a customer changes the behaviour or interface of an existing service with his extension the breaking change was caused by him and he has to deal with the implications.
