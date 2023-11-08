## Continuous Delivery p1.
![image](https://github.com/gurustron/stream-notes/assets/6535969/1dab0f1f-f401-4c4d-9815-a19419857b94)

- subvert tools (use tools "createvely" to automate stuff)
- timebox automation
- don't shave yaks =)
- yesterday best practices is tomorrow antipattern

## Applying abstraction

Levels of abstraction:
![image](https://github.com/gurustron/stream-notes/assets/6535969/4db0be1f-023c-45f3-a098-6ff495271253)

Methods of abstraction:

![image](https://github.com/gurustron/stream-notes/assets/6535969/b377f232-8262-4535-ae5b-4fa9cdcb8d47)

|             | location transparency | name transparency | implementation transparency | access decoupling | contract decoupling |
|-------------|-----------------------|-------------------|-----------------------------|-------------------|---------------------|
| messaging   |           V           |         V         |              V              |         X         |          X          |
| adapter     |           V           |         V         |              V              |         V         |          V          |
| message bus |           V           |         V         |              V              |         V         |          V          |

## Choosing and Comparing Architectures

### Normalize feature sets
- Build your own feature matrix
- good, bad ugly

### Suck/rock dichotomy 

### Spikes
- time-boxed experimental coding exercise
- pure knowledge acquisition
- NOT a prototype
- develop on "dead end" branch

### CQRS

Natural fits:
- task-based UI
- meshes well with event sourcing
- eventual consistency (consistency or availability, never both)
- complex or granular domains
  
https://www.allthingsdistributed.com/2007/12/eventually_consistent.html
https://www.allthingsdistributed.com/2008/12/eventually_consistent.html

### Domain logic classification
https://martinfowler.com/bliki/EagerReadDerivation.html

- validations: checks that input makes sense and objects are properly suited for further actions.
- consequences: initiating some action that will change the state of the world
- derivations: figuring out some information based on information we already have

### LMAX

https://martinfowler.com/articles/lmax.html

![image](https://github.com/gurustron/stream-notes/assets/6535969/8d945ecf-c506-432a-a2e7-171845187114)

At a top level, the architecture has three parts
- business logic processor
- input disruptor
- output disruptors

### Architect for change vs YAGNI

## 5. Web Services and Messaging

- REST in Practice by Jim Webber, Savas Parastatidis, Ian Robinson
- [Richardson Maturity Model](https://martinfowler.com/articles/richardsonMaturityModel.html)
- HATEOFS: Hypermedia as the engine of application state

## 6. SOA Fundamentals

### SOA must haves

![image](https://github.com/gurustron/stream-notes/assets/6535969/1a4dc674-4162-4daa-9aa3-deb9360c7790)


- Business services (services _business needs_ to perfrom)
- Service abstraction (usually all 5 should be achieved):
  - location transparency
  - name transparency
  - implementation transparency
  - access decoupling
  - contract decoupling

Service registry can access first 3 levels of abstraction.

![image](https://github.com/gurustron/stream-notes/assets/6535969/bf24984e-e86b-4c08-a67c-f89975569162)

### SOA core pattern elements:
- service registry 
- message enhancement
- message transformation
- protocol transformation

## 7. Integration hubs


