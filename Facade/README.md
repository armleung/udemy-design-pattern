## Facade (Class structure)

![Image](https://refactoring.guru/images/patterns/content/facade/facade.png?id=1f4be17305b6316fbd548edf1937ac3b)

### :octocat: Intent
>Facade is a **structural** design pattern that provides a simplified interface to a library, a framework, or any other complex set of classes.

### :octocat: Structure
![Image](https://refactoring.guru/images/patterns/diagrams/facade/structure.png?id=258401362234ac77a2aaf1cde62339e7)

### :octocat: Applicability 
- Use the Facade pattern when you need to have a limited but straightforward interface to a complex subsystem.
- Use the Facade when you want to structure a subsystem into layers.

### :octocat: Consequences
- You can isolate your code from the complexity of a subsystem.

### :octocat: Related Patterns
- [Flyweight](https://github.com/armleung/udemy-design-pattern/tree/master/Flyweight) shows how to make lots of little objects, whereas Facade shows how to make a single object that represents an entire subsystem.
- A Facade class can often be transformed into a [Singleton](https://github.com/armleung/udemy-design-pattern/tree/master/Sigleton) since a single facade object is sufficient in most cases.