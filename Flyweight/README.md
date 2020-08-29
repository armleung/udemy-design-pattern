## Flyweight Pattern (Object Structural)
![Image](https://refactoring.guru/images/patterns/content/flyweight/flyweight.png)

### :octocat: Intent
>Flyweight is a structural design pattern that lets you fit more objects into the available amount of RAM by sharing common parts of state between multiple objects instead of keeping all of the data in each object.

### :octocat: Structure
![Image](https://refactoring.guru/images/patterns/diagrams/flyweight/structure.png)

### :octocat: Applicability 
The benefit of applying the pattern depends heavily on how and where it’s used. It’s most useful when:
- an application needs to spawn a huge number of similar objects
- this drains all available RAM on a target device
- the objects contain duplicate states which can be extracted and shared between multiple objects

### :octocat: Consequences
- You can save lots of RAM, assuming your program has tons of similar objects.
- You might be trading RAM over CPU cycles when some of the context data needs to be recalculated each time somebody calls a flyweight method.
- The code becomes much more complicated. New team members will always be wondering why the state of an entity was separated in such a way.

### :octocat: Related Patterns
- [Composite](https://github.com/armleung/udemy-design-pattern/tree/master/Composite)
    - You can implement shared leaf nodes of the Composite tree as Flyweights to save some RAM.

### :octocat: Implementation
```cpp
struct User
{
  User(const string& first_name, const string& last_name)
    : first_name{add(first_name)}, last_name{add(last_name)}
  {
  }

  const string& get_first_name() const
  {
    return names.left.find(last_name)->second;
  }

  const string& get_last_name() const
  {
    return names.left.find(last_name)->second;
  }

  static void info()
  {
    for (auto entry : names.left)
    {
      cout << "Key: " << entry.first << ", Value: " << entry.second << endl;
    }
  }

  friend ostream& operator<<(ostream& os, const User& obj)
  {
    return os
      << "first_name: " << obj.first_name << " " << obj.get_first_name()
      << " last_name: " << obj.last_name << " " << obj.get_last_name();
  }

protected:
  static bimap<key, string> names;
  static int seed;

  static key add(const string& s)
  {
    auto it = names.right.find(s);
    if (it == names.right.end())
    {
      // add it
      key id = ++seed;
      names.insert(bimap<key, string>::value_type(seed, s));
      return id;
    }
    return it->second;
  }
  key first_name, last_name;
};
```