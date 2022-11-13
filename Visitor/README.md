## Visitor (Behavioral)

![Image](https://refactoring.guru/images/patterns/content/visitor/visitor.png?id=f36d100188340db7a18854ef7916f972)

### :octocat: Intent

>**Visitor** is a behavioral design pattern that lets you separate algorithms from the objects on which they operate.

### :octocat: Structure

![Image](https://refactoring.guru/images/patterns/diagrams/visitor/structure-en.png?id=34126311c4e0d5c9fbb970595d2f1777)

### :octocat: Applicability 
- Use the Visitor when you need to perform an operation on all elements of a complex object structure (for example, an object tree).
- Use the Visitor to clean up the business logic of auxiliary behaviors.
- Use the pattern when a behavior makes sense only in some classes of a class hierarchy, but not in others.

### :octocat: Consequences

- Open/Closed Principle. You can introduce a new behavior that can work with objects of different classes without changing these classes.
- Single Responsibility Principle. You can move multiple versions of the same behavior into the same class.
- A visitor object can accumulate some useful information while working with various objects. This might be handy when you want to traverse some complex object structure, such as an object tree, and apply the visitor to each object of this structure.

### :octocat: Related Patterns
- You can treat Visitor as a powerful version of the [Command](https://github.com/armleung/udemy-design-pattern/tree/master/Command) pattern. Its objects can execute operations over various objects of different classes.
- You can use Visitor to execute an operation over an entire [Composite](https://github.com/armleung/udemy-design-pattern/tree/master/Composite) tree.
- You can use Visitor along with [Iterator](https://github.com/armleung/udemy-design-pattern/tree/master/Iterator) to traverse a complex data structure and execute some operation over its elements, even if they all have different classes.