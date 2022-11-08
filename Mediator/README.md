## Mediator (Object Behavioral)

![Image](https://refactoring.guru/images/patterns/content/mediator/mediator.png?id=0264bd857a231b6ea2d0c537c092e698)

### :octocat: Intent

>**Mediator** is a behavioral design pattern that lets you reduce chaotic dependencies between objects. The pattern restricts direct communications between the objects and forces them to collaborate only via a mediator object.

### :octocat: Structure

![Image](https://refactoring.guru/images/patterns/diagrams/mediator/structure.png?id=1f2accc7820ecfe9665b6d30cbc0bc61)

### :octocat: Applicability 
- Use the Mediator pattern when it’s hard to change some of the classes because they are tightly coupled to a bunch of other classes.
- Use the pattern when you can’t reuse a component in a different program because it’s too dependent on other components.

### :octocat: Consequences
- Single Responsibility Principle. You can extract the communications between various components into a single place, making it easier to comprehend and maintain.
- Open/Closed Principle. You can introduce new mediators without having to change the actual components.

### :octocat: Related Patterns
- [Chain of Responsibility](https://github.com/armleung/udemy-design-pattern/tree/master/Chain%20of%20Responsibility), [Command](https://github.com/armleung/udemy-design-pattern/tree/master/Command), Mediator and [Observer](https://github.com/armleung/udemy-design-pattern/tree/master/Observer) address various ways of connecting senders and receivers of requests:
  - [Chain of Responsibility](https://github.com/armleung/udemy-design-pattern/tree/master/Chain%20of%20Responsibility) passes a request sequentially along a dynamic chain of potential receivers until one of them handles it.
  - [Command](https://github.com/armleung/udemy-design-pattern/tree/master/Command) establishes unidirectional connections between senders and receivers.
  - Mediator eliminates direct connections between senders and receivers, forcing them to communicate indirectly via a mediator object.
  - [Observer](https://github.com/armleung/udemy-design-pattern/tree/master/Observer) lets receivers dynamically subscribe to and unsubscribe from receiving requests.
- [Facade](https://github.com/armleung/udemy-design-pattern/tree/master/Facade) and Mediator have similar jobs: they try to organize collaboration between lots of tightly coupled classes.
  - [Facade](https://github.com/armleung/udemy-design-pattern/tree/master/Facade) defines a simplified interface to a subsystem of objects, but it doesn’t introduce any new functionality. The subsystem itself is unaware of the facade. Objects within the subsystem can communicate directly.
  - Mediator centralizes communication between components of the system. The components only know about the mediator object and don’t communicate directly.

The difference between Mediator and Observer is often elusive. In most cases, you can implement either of these patterns; but sometimes you can apply both simultaneously. Let’s see how we can do that.

The primary goal of Mediator is to eliminate mutual dependencies among a set of system components. Instead, these components become dependent on a single mediator object. The goal of Observer is to establish dynamic one-way connections between objects, where some objects act as subordinates of others.

There’s a popular implementation of the Mediator pattern that relies on Observer. The mediator object plays the role of publisher, and the components act as subscribers which subscribe to and unsubscribe from the mediator’s events. When Mediator is implemented this way, it may look very similar to Observer.

When you’re confused, remember that you can implement the Mediator pattern in other ways. For example, you can permanently link all the components to the same mediator object. This implementation won’t resemble Observer but will still be an instance of the Mediator pattern.

Now imagine a program where all components have become publishers, allowing dynamic connections between each other. There won’t be a centralized mediator object, only a distributed set of observers.

### :octocat: Implementation
```cpp
#include <iostream>
#include <vector>
using namespace std;

struct IParticipant
{
    virtual void notify(IParticipant* sender, int value) = 0;
};

struct Mediator
{
    vector<IParticipant*> participants;
    void say(IParticipant* sender, int value)
    {
      for (auto p : participants)
        p->notify(sender, value);
    }
};

struct Participant : IParticipant
{
    int value{0};
    Mediator& mediator;

    Participant(Mediator &mediator) : mediator(mediator)
    {
      mediator.participants.push_back(this);
    }

    void notify(IParticipant *sender, int value) override {
      if (sender != this)
        this->value += value;
    }

    void say(int value)
    {
      mediator.say(this, value);
    }
};
```