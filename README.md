# Notes on Domain Driven Design

## Part I: Putting the Domain Model to Work

### Chapter 1: Crunching Knowledge

- create a model by having a back-and-forth with domain experts
- determine what the key components of the domain are and how they need to interact
- knowledge crunching is the distillation of the complex concepts, processes, and terminology of the domain into
a simple, rigorous model that captures the important parts
- developers need to be continuously learning about the domain that they are working with

### Chapter 2: Communication and the Use of Language

- the design of the software project should develop a language that is used for all conversations among developers,
domain experts, managers, etc
- if everybody is using the same language, the concepts become more solidified in everyone's head
- developers should not have to translate when talking to domain experts
- the implementation should be a direct reflection of the way the system is talked about
- a slight change in the business rules generally shouldn't require a drastic change in the software system because
implementation details are different from the domain model
- refine the model through conversations. If speaking about the model is awkward or doesn't make sense, try to talk
about it in a way that makes more sense, and then refactor the software to match that new language
- "if domain experts don't understand the model, there is something wrong with the model"
- UML can be used to anchor a discussion, but you don't need to get too crazy with it and try to lay out your entire
architecture in it
  - maybe use it to illustrate certain subsystems

### Chapter 3: Binding Model and Implementation

- "Tightly relating the code to an underlying model gives the code meaning and makes the model relevant"
- the model must both capture the essence of the system and be able to be implemented in software
- PCB net/bus example
  - PCB layout tool doesn't understand buses
  - engineers write script to parse PCB tool output file and rule file and generate output file with bus rules inserted
  - no knowledge or model of the domain is used
  - if anything changed, the whole script would be useless
  - its also very hard to test
- we should expose the model to users
  - "trying to create in the UI an illusion of a model other than the domain model will cause confusion unless the illusion is perfect"
- when the model matches the implementation, if we start to think of new and interesting ways the domain objects can interact,
implementing the change should be relatively straightforward, whereas if the domain model is not present in the code, using the domain
objects in different ways may not be possible because the objects do not even exist in the code necessarily

## Part II: The Building Blocks of a Model-Driven Design

### Chapter 4: Isolating the Domain

- section of the software dealing with domain should be insulated/isolated from the rest of the system
- domain logic should not care about any software technology details
- sometimes, UI, database, and random support code is written into business objects and business logic can leak into UI code, support code, etc
  - makes the system very hard to understand
  - we need to separate concerns
- layered architecture - layers can only depend on things in same layer or beneath
- for example: UI, Application, Domain, and Infra layers
- "elaborate frameworks can also straightjacket application developers"
- avoid tight coupling of the implementation and the framework to allow for flexibility
- Key: ISOLATE THE DOMAIN IMPLEMENTATION
- Smart UI (not really developing a domain model and just mangling everything together) is appropriate sometimes when a project is simple
and needs to be delivered fast. It will become unwieldy if the project grows.
- there are also other forces that can "corrupt" the isolated domain layer, that will be discussed in Chapter 14

### Chapter 5: A Model Expressed in Software

- this chapter discusses how to connect the model and implementation
- implementing associations is sometimes confusing (very true)
- three patterns of model elements
  1. Entities (something with an identity that may be tracked through different states)
  2. Value Objects (describes the state of something else)
  3. Services (business rule best described as a action/operation)
- bidirectional association means that both objects can only be understood together
- Ex: The USA has had many presidents (one to many relationship)
  - We don't really need to ask "Of what country was George Washington president?"
  - We only need to understand this relationship from Country --> President, not President --> Country
- Ex: A Bank Account can explicitly store a Customer object
  - A Bank Account can return a Customer object from a method which uses a stored Customer ID or SSN to query a database and return a Customer
  - A Bank Account can return all investments associated with it
  - A Bank Account can return one investment identified by a stock ticker (qualifying the association)

#### Entities (A.K.A Reference Objects)

- certain objects persist and have identities
- people, for example, have many properties that may change but they retain the same "identity"
- an entity is an object defined primarily by its identity
- two transactions to a bank account could each be their own entities
  - the amount attributes of those transactions are not entities
- basic responsibility is to establish continuity
  - they should be kept very simple otherwise
- they should fulfill responsibilities by coordinating operations of objects they own
- there are some situations where establishing identity is challenging
  - establishing identity across software systems (could use SSN or telephone number but there are always edge cases)

#### Value Objects