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
