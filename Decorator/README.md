## Decorator Pattern (Object Structural)

![Image](https://refactoring.guru/images/patterns/content/decorator/decorator.png)
### Intent
> Attaches additional responsibilities to an **object** dynamically. Decorators provide a flexible alternative to subclassing for extending functionality.

### Structure
![Image](https://refactoring.guru/images/patterns/diagrams/decorator/structure-indexed.png)

### Applicability 
- Use the Decorator pattern when you need to be able to assign extra behaviors to objects at runtime without breaking the code that uses these objects.
- Use the pattern when it’s awkward or not possible to extend an object’s behavior using inheritance.
- Many programming languages have the final keyword that can be used to prevent further extension of a class. For a final class, the only way to reuse the existing behavior would be to wrap the class with your own wrapper, using the Decorator pattern.

### Consequences
- More flexibility than static inheritance
- Decorator and its components aren't identical
- Lots of little objects

### Related Patterns
- [Adapter](https://github.com/armleung/udemy-design-pattern/tree/master/Adapter)
    - **Adapter** change the interface of existing object VS **Decorator** enhance an object without changing its interface
    - **Adapter** provides a different interface to the wrapped object, **Proxy** provides it with the same interface, and **Decorator** provides it with an enhanced interface
- [Composite](https://github.com/armleung/udemy-design-pattern/tree/master/Composite)
    - **Composite** and **Decorator** have similar structure diagrams since both rely on recursive composition to organize an open-ended number of objects.
    - **Decorator** adds additional responsibilities to the wrapped object, while **Composite** just “sums up” its children’s results.
- [Strategy](https://github.com/armleung/udemy-design-pattern/tree/master/Strategy)
    - **Decorator** lets you change the skin of an object, while **Strategy** lets you change the guts.
### Implementation
#### Dynamic Decorator
```cpp
struct Shape
{
  virtual string str() const = 0;
};

struct Circle : Shape
{
  float radius;

  Circle(){}

  explicit Circle(const float radius)
    : radius{radius}
  {
  }

  void resize(float factor)
  {
    radius *= factor;
  }

  string str() const override
  {
    ostringstream oss;
    oss << "A circle of radius " << radius;
    return oss.str();
  }
};

struct Square : Shape
{
  float side;

  Square(){}

  explicit Square(const float side)
    : side{side}
  {
  }

  string str() const override
  {
    ostringstream oss;
    oss << "A square of with side " << side;
    return oss.str();
  }
};

// we are not changing the base class of existing
// objects

// cannot make, e.g., ColoredSquare, ColoredCircle, etc.

struct ColoredShape : Shape
{
  Shape& shape;
  string color;

  ColoredShape(Shape& shape, const string& color)
    : shape{shape},
      color{color}
  {
  }

  string str() const override
  {
    ostringstream oss;
    oss << shape.str() << " has the color " << color;
    return oss.str();
  }
};

struct TransparentShape : Shape
{
  Shape& shape;
  uint8_t transparency;


  TransparentShape(Shape& shape, const uint8_t transparency)
    : shape{shape},
      transparency{transparency}
  {
  }

  string str() const override
  {
    ostringstream oss;
    oss << shape.str() << " has "
      << static_cast<float>(transparency) / 255.f*100.f
      << "% transparency";
    return oss.str();
  }
};
void wrapper()
{
  Circle circle{ 5 };
  cout << circle.str() << endl;

  ColoredShape red_circle{ circle, "red" };
  cout << red_circle.str() << endl;

  //red_circle.resize(); // oops

  TransparentShape red_half_visible_circle{ red_circle, 128 };
  cout << red_half_visible_circle.str() << endl;
}
```
#### Static Decorator
```cpp
// mixin inheritance

// note: class, not typename
template <typename T> struct ColoredShape2 : T
{
  static_assert(is_base_of<Shape, T>::value,
    "Template argument must be a Shape");

  string color;

  // need this (or not!)
  ColoredShape2(){}

  template <typename...Args>
  ColoredShape2(const string& color, Args ...args)
    : T(std::forward<Args>(args)...), color{color}
    // you cannot call base class ctor here
    // b/c you have no idea what it is
  {
  }

  string str() const override
  {
    ostringstream oss;
    oss << T::str() << " has the color " << color;
    return oss.str();
  }
};

template <typename T> struct TransparentShape2 : T
{
  uint8_t transparency;

  template<typename...Args>
  TransparentShape2(const uint8_t transparency, Args ...args)
    : T(std::forward<Args>(args)...), transparency{ transparency }
  {
  }

  string str() const override
  {
    ostringstream oss;
    oss << T::str() << " has "
      << static_cast<float>(transparency) / 255.f * 100.f
      << "% transparency";
    return oss.str();
  }
};
void mixin_inheritance()
{
  // won't work without a default constructor
  ColoredShape2<Circle> green_circle{ "green" };
  green_circle.radius = 123;
  cout << green_circle.str() << endl;

  TransparentShape2<ColoredShape2<Square>> blue_invisible_square{ 0 };
  blue_invisible_square.color = "blue";
  blue_invisible_square.side = 321;
  cout << blue_invisible_square.str() << endl;
}
```
#### Functional Decorator
```cpp
struct Logger
{
  function<void()> func;
  string name;

  Logger(const function<void()>& func, const string& name)
    : func{func},
      name{name}
  {
  }

  void operator()() const
  {
    cout << "Entering " << name << endl;
    func();
    cout << "Exiting " << name << endl;
  }
};

template <typename Func>
struct Logger2
{
  Func func;
  string name;

  Logger2(const Func& func, const string& name)
    : func{func},
      name{name}
  {
  }

  void operator()() const
  {
    cout << "Entering " << name << endl;
    func();
    cout << "Exiting " << name << endl;
  }
};

template <typename Func> auto make_logger2(Func func, 
  const string& name)
{
  return Logger2<Func>{ func, name }; 
}

// need partial specialization for this to work
template <typename> struct Logger3;

// return type and argument list
template <typename R, typename... Args> 
struct Logger3<R(Args...)>
{
  Logger3(function<R(Args...)> func, const string& name)
    : func{func},
      name{name}
  {
  }

  R operator() (Args ...args)
  {
    cout << "Entering " << name << endl;
    R result = func(args...);
    cout << "Exiting " << name << endl;
    return result;
  }

  function<R(Args ...)> func;
  string name;
};

template <typename R, typename... Args>
auto make_logger3(R (*func)(Args...), const string& name)
{
  return Logger3<R(Args...)>(
    std::function<R(Args...)>(func), 
    name);
}

double add(double a, double b)
{
  cout << a << "+" << b << "=" << (a + b) << endl;
  return a + b;
}

void function_decorator()
{
  //Logger([]() {cout << "Hello" << endl; }, "HelloFunction")();
  
  // cannot do this
  //make_logger2([]() {cout << "Hello" << endl; }, "HelloFunction")();
  auto call = make_logger2([]() {cout << "Hello!" << endl; }, "HelloFunction");
  call();

  auto logged_add = make_logger3(add, "Add");
  auto result = logged_add(2, 3);
}
```