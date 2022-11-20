## Interpreter (Class Behavioral)

### :octocat: Intent

>**Interpreter** is defining a representation for its grammer along with an interpreter that uses the representatino to interpret sentences in the language.

### :octocat: Structure

![Imnage](https://www.baeldung.com/wp-content/uploads/2018/07/Interpreter.png)

### :octocat: Applicability 
The interpreter pattern works best when 
- The grammar is simple. For complex grammars, the class hierarchy for the grammar becomes large and unmanageable.
- Efficiency is not a critical concern. The most efficient interpreter are usually not implemented by interpreting parse trees directly but by first translating them into another form.
### :octocat: Consequences
- It's easy to change and extend the grammar
- Implementing the grammar is easy, too
- Complex grammars are hard to maintain
- Adding new ways to interpret expressions

### :octocat: Related Patterns
- [Composite](https://github.com/armleung/udemy-design-pattern/tree/master/Composite):
    - The abstract syntax tree is an instance of the composite pattern.
- [Flyweight](https://github.com/armleung/udemy-design-pattern/tree/master/Flyweight):
    - It shows how to share terminal symbols within the abstract syntax tree.
- [Iterator](https://github.com/armleung/udemy-design-pattern/tree/master/Iterator):
    - The interpreter can use an Iterator to traverse the structure.
- [Visitor](https://github.com/armleung/udemy-design-pattern/tree/master/Visitor):
    - It can be used to maintain the behavior in each node in the abstract syntax tree in one class.

### :octocat: Implementation