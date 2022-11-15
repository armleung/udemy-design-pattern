## Strategy (Behavioral)
![Image](https://refactoring.guru/images/patterns/content/strategy/strategy.png?id=379bfba335380500375881a3da6507e0)

### :octocat: Intent
>**Strategy** is a behavioral design pattern that lets you define a family of algorithms, put each of them into a separate class, and make their objects interchangeable.

### :octocat: Structure
![Image](https://refactoring.guru/images/patterns/diagrams/strategy/structure.png?id=c6aa910c94960f35d100bfca02810ea1)

### :octocat: Applicability 
- Use the Strategy pattern when you want to use different variants of an algorithm within an object and be able to switch from one algorithm to another during runtime.
- Use the Strategy when you have a lot of similar classes that only differ in the way they execute some behavior.
- Use the pattern to isolate the business logic of a class from the implementation details of algorithms that may not be as important in the context of that logic.
- Use the pattern when your class has a massive conditional statement that switches between different variants of the same algorithm.
### :octocat: Consequences
- You can swap algorithms used inside an object at runtime.
- You can replace inheritance with composition.
- *Open/Closed* Principle. You can introduce new strategies without having to change the context.
### :octocat: Related Patterns
- [Decorator](https://github.com/armleung/udemy-design-pattern/tree/master/Decorator) lets you change the skin of an object, while Strategy lets you change the guts.
- [Template](https://github.com/armleung/udemy-design-pattern/tree/master/Template%20Method) Method is based on inheritance: it lets you alter parts of an algorithm by extending those parts in subclasses. Strategy is based on composition: you can alter parts of the object’s behavior by supplying it with different strategies that correspond to that behavior. Template Method works at the class level, so it’s static. Strategy works on the object level, letting you switch behaviors at runtime.

### :octocat: Implementation