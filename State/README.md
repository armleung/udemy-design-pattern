## State (Behavioral)
![Image](https://refactoring.guru/images/patterns/content/state/state-en.png?id=c323fb8c54e2d57bebf4806c087afb07)

### :octocat: Intent
>**State** is a behavioral design pattern that lets an object alter its behavior when its internal state changes. It appears as if the object changed its class.

### :octocat: Structure
![Image](https://refactoring.guru/images/patterns/diagrams/state/structure-en.png?id=38c5cc3a610a201e5bc26a441c63d327)

### :octocat: Applicability 
- *Single Responsibility* Principle. Organize the code related to particular states into separate classes.
- *Open/Closed* Principle. Introduce new states without changing existing state classes or the context.
- Simplify the code of the context by eliminating bulky state machine conditionals.

### :octocat: Related Patterns
- State can be considered as an extension of [Strategy](https://github.com/armleung/udemy-design-pattern/tree/master/Stragegy). Both patterns are based on composition: they change the behavior of the context by delegating some work to helper objects. Strategy makes these objects completely independent and unaware of each other. However, State doesn’t restrict dependencies between concrete states, letting them alter the state of the context at will.