## # Ch 5 Designing business logic in a microservice architecture

### Intro

- The Aggregate pattern structures a service's business logic as a collection of aggregates.

- An aggregate is a cluster of objects that can be treated as a unit

  - avoid possibility of object references spanning service boundaries (pass primary key values rather than object references)

  - Transaction can only create or update a single aggregate

### 5.1 Business logic organization patterns

  - An inbound adapter handles requests from clients and invokes the business logic

  - An outbound adapter (invoked by business logic) invokes other services & apps

  - **Transaction script pattern**

    - write a method called a transaction script to handle request from presentation tier (procedural code, when OO is OVERkill)

    - one set of classes implements behavior and another stores state

    - scripts usually located in service classes, has one method for each request

    - script accesses the db using data access objects (DAOs)

    - good for simple business logic, not good for complex

  -** Domain model pattern**

    - object model, network of relatively small classes

    - closely mirror the real world

    - easier to test and extend

    - some classes have only state state or behavior, many contain both

  - **Domain-driven design**

    - *Entity* -- object that has a persistent identity

    - *Value object* -- object that is a collection of values

    - *Factory* -- object or method that implements object creation logic that's too complex to be done directly by a constructor. can hide concrete classes that are instantiated

    - *Repository* -- object that provides access to persistent entities

    - *Service* -- object that implements business logic that doesn't belong in an entity or value object

    - *Aggregate* -- a cluster of domain objects within a boundary that can be treated as a unit

### 5.2 Designing a domain model using the DDD aggregate pattern



*   Lack of explicit boundaries causes problems when updating business objects
    *   Business objects have invariants (rules that must be enforced) 
*   optimistic vs pessimistic locking
*   pessimistic: locked at moment of access, nothing else can access
*   optimistic: state saved at moment of access, record re-read before commit, state compared, if changed, rollback

  **- An aggregate is a cluster of domain objects within a boundary that can be treated as a unit**

**    - consists of a root entity and one or more entities and value objects e.g. Order, Consumer, Restaurant**

    - decompose domain model into chunks, clarify scope of operations

    - often loaded in its entirety from db

  - Aggregate rules

    - #1) reference only the aggregate root

    - #2) aggregates reference each other by identity instead of object references

    - #3) one transaction creates or updates the aggregate

### 5.3 Publishing domain events

  - something that has happened to an aggregate, represented by class in the domain model

  - property has primitive value or a value object, metadata, event ID and a timestamp

  - event enrichment: events contain information that consumers need

  - split responsibility of publishing events between aggregate and the service that invokes it

  - aggregate generates the events when it changes state and returns them to the service

  - could also have aggregate root accumulate events in a field and have service retrieve and publish them

  - consuming domain events

    - domain events are published as messages to message broker (eg. Kafka)  


<p class='util--hide'>View <a href='https://learn.co/lessons/microservices-patterns-chapter-5'>Microservices Patterns Chapter 5</a> on Learn.co and start learning to code for free.</p>
