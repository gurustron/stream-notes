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

Suck/rock dichotomy 

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
