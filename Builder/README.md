## Builder Pattern (Creational)
![Image](https://refactoring.guru/images/patterns/content/builder/builder-en.png)

### :octocat: Intent
> **Builder** is a creational design pattern that lets you construct complex objects step by step. The pattern allows you to produce different types and representations of an object using the same construction code.

### :octocat: Structure
![Image](https://refactoring.guru/images/patterns/diagrams/builder/structure.png)

### :octocat: Applicability 
- The Builder pattern lets you build objects step by step, using only those steps that you really need. After implementing the pattern, you don’t have to cram dozens of parameters into your constructors anymore.
- The Builder pattern can be applied when construction of various representations of the product involves similar steps that differ only in the details.
- A builder doesn’t expose the unfinished product while running construction steps. This prevents the client code from fetching an incomplete result.

### :octocat: Consequences
- Single Responsibility Principle. You can isolate complex construction code from the business logic of the product.
- You can construct objects step-by-step, defer construction steps or run steps recursively.

### :octocat: Related Patterns
- [Abstract Factory](https://github.com/armleung/udemy-design-pattern/tree/master/Abstract Factory)
    - **Builder** focuses on constructing complex objects step by step. **Abstract Factory** specializes in creating families of related objects. **Abstract Factory** returns the product immediately, whereas **Builder** lets you run some additional construction steps before fetching the product.

### :octocat: Implementation
1. Simple Builder 
```cpp
struct HtmlBuilder
{
  HtmlBuilder(string root_name)
  {
    root.name = root_name;
  }

  // void to start with
  HtmlBuilder& add_child(string child_name, string child_text)
  {
    HtmlElement e{ child_name, child_text };
    root.elements.emplace_back(e);
    return *this;
  }

  // pointer based
  HtmlBuilder* add_child_2(string child_name, string child_text)
  {
    HtmlElement e{ child_name, child_text };
    root.elements.emplace_back(e);
    return this;
  }

  string str() { return root.str(); }

  operator HtmlElement() const { return root; }
  HtmlElement root;
};

int demo()
{
    // easier
  HtmlBuilder builder{ "ul" };
  builder.add_child("li", "hello").add_child("li", "world");
  cout << builder.str() << endl;


  auto builder2 = HtmlElement::build("ul")
    ->add_child_2("li", "hello")->add_child_2("li", "world");
  cout << builder2 << endl;

  getchar();
  return 0;
}
```
2. Groovy Builder 
```cpp

namespace html {
  struct Tag
  {
    std::string name;
    std::string text;
    std::vector<Tag> children;
    std::vector<std::pair<std::string, std::string>> attributes;

    friend std::ostream& operator<<(std::ostream& os, const Tag& tag)
    {
      os << "<" << tag.name;

      for (const auto& att : tag.attributes)
        os << " " << att.first << "=\"" << att.second << "\"";

      if (tag.children.size() == 0 && tag.text.length() == 0)
      {
        os << "/>" << std::endl;
      } 
      else
      {
        os << ">" << std::endl;

        if (tag.text.length())
          os << tag.text << std::endl;

        for (const auto& child : tag.children)
          os << child;

        os << "</" << tag.name << ">" << std::endl;
      }

      return os;
    }
  protected:

    Tag(const std::string& name, const std::string& text)
      : name{name},
        text{text}
    {
    }


    Tag(const std::string& name, const std::vector<Tag>& children)
      : name{name},
        children{children}
    {
    }
  };

  struct P : Tag
  {
    explicit P(const std::string& text)
      : Tag{"p", text}
    {
    }

    P(std::initializer_list<Tag> children)
      : Tag("p", children)
    {
    }
    
  };

  struct IMG : Tag
  {
    explicit IMG(const std::string& url)
      : Tag{"img", ""}
    {
      attributes.emplace_back(make_pair("src", url));
    }
  };
}

int main1()
{
  using namespace html;

  std::cout <<

    P {
      IMG {"http://pokemon.com/pikachu.png"}
    }

    << std::endl;

  getchar();
  return 0;
}
```
3. ![Facets Builder](https://github.com/armleung/udemy-design-pattern/tree/master/Builder/BuilderFacets)
