## Adapter Pattern (Class Object Structural)

![Image](https://refactoring.guru/images/patterns/content/adapter/adapter-en.png)

### :octocat: Intent

>**Adapter** is a structural design pattern that allows objects with incompatible interfaces to collaborate.

### :octocat: Structure
1. Object Adapter (uses the object composition)

![Image](https://refactoring.guru/images/patterns/diagrams/adapter/structure-object-adapter.png)

2. Class Adapter (uses multi inheritance)

![Image](https://refactoring.guru/images/patterns/diagrams/adapter/structure-class-adapter.png)

### :octocat: Applicability 
- Use the Adapter class when you want to use some existing class, but its interface isn’t compatible with the rest of your code.
- The Adapter pattern lets you create a middle-layer class that serves as a translator between your code and a legacy class, a 3rd-party class or any other class with a weird interface.

### :octocat: Consequences
- Class Adapter 
    - Committing to a concrete Adaptee class. Won't work when we want to adapt a class and all its subclass
    - Adapter override some of Adaptee's behavior
- Object Adapter
    - Single Adapter work with many Adaptees
    - Harder to override Adaptee behavior

### :octocat: Related Patterns
- [Bridge](https://github.com/armleung/udemy-design-pattern/tree/master/Bridge)
    - **Bridge** is usually designed up-front, letting you develop parts of an application independently of each other. On the other hand, **Adapter** is commonly used with an existing app to make some otherwise-incompatible classes work together nicely.
- [Decorator](https://github.com/armleung/udemy-design-pattern/tree/master/Decorator)
    - **Adapter** changes the interface of an existing object, while **Decorator** enhances an object without changing its interface. In addition, **Decorator** supports recursive composition, which isn’t possible when you use **Adapter**.
- [Proxy](https://github.com/armleung/udemy-design-pattern/tree/master/Proxy)
    - **Adapter** provides a different interface to the wrapped object, **Proxy** provides it with the same interface, and **Decorator** provides it with an enhanced interface.

### :octocat: Implementation
```cpp
struct Square
{
  int side{ 0 };


  explicit Square(const int side)
    : side(side)
  {
  }
};

struct Rectangle
{
  virtual int width() const = 0;
  virtual int height() const = 0;

  int area() const
  {
    return width() * height();
  }
};

struct SquareToRectangleAdapter : Rectangle
{
  SquareToRectangleAdapter(const Square& square)
    : square(square) {}
  
  int width() const override
  {
    return square.side;
  }

  int height() const override
  {
    return square.side;
  }

  const Square& square;
};
```