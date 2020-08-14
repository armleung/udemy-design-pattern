## Chain of Responsibility Pattern (Behavioral Pattern)
![Image](https://refactoring.guru/images/patterns/content/chain-of-responsibility/chain-of-responsibility.png)
### :octocat: Intent
>**Chain of Responsibility** is a behavioral design pattern that lets you pass requests along a chain of handlers. Upon receiving a request, each handler decides either to process the request or to pass it to the next handler in the chain.

### :octocat: Structure
![Image](https://refactoring.guru/images/patterns/diagrams/chain-of-responsibility/structure.png)

### :octocat: Applicability 

- Use the pattern when it’s essential to execute several handlers in a **particular order**.
- Use the CoR pattern when the set of handlers and their order are supposed to **change at runtime**.

### :octocat: Consequences
- Reduced coupling
- Added flexibility in assigning responsibilities to objects
- Receipt isn't guaranteed

### :octocat: Related Patterns
- [Composite](https://github.com/armleung/udemy-design-pattern/tree/master/Composite)
    - **Chain of Responsibility** is often used in conjunction with **Composite**. In this case, when a leaf component gets a request, it may pass it through the chain of all of the parent components down to the root of the object tree.
- [Decorator](https://github.com/armleung/udemy-design-pattern/tree/master/Decorator)
    - **Chain of Responsibility** and Decorator have very similar class structures. Both patterns rely on recursive composition to pass the execution through a series of objects. However, there are several crucial differences.

### :octocat: Implementation
1. Pointer Chain
```cpp
struct Creature
{
  string name;
  int attack, defense;

  Creature(const string& name, const int attack, const int defense)
    : name(name),
      attack(attack),
      defense(defense)
  {
  }


  friend ostream& operator<<(ostream& os, const Creature& obj)
  {
    return os
      << "name: " << obj.name
      << " attack: " << obj.attack
      << " defense: " << obj.defense;
  }
};

class CreatureModifier
{
  CreatureModifier* next{ nullptr }; // unique_ptr
protected:
  Creature& creature; // pointer or shared_ptr
public:
  explicit CreatureModifier(Creature& creature)
    : creature(creature)
  {
  }
  virtual ~CreatureModifier() = default;

  void add(CreatureModifier* cm)
  {
    if (next) next->add(cm);
    else next = cm;
  }

  // two approaches:

  // 1. Always call base handle(). There could be additional logic here.
  // 2. Only call base handle() when you cannot handle things yourself.

  virtual void handle()
  {
    if (next) next->handle();
  }
};

// 1. Double the creature's attack
// 2. Increase defense by 1 unless power > 2
// 3. No bonuses can be applied to this creature

class NoBonusesModifier : public CreatureModifier
{
public:
  explicit NoBonusesModifier(Creature& creature)
    : CreatureModifier(creature)
  {
  }

  void handle() override
  {
    // nothing
  }
};

class DoubleAttackModifier : public CreatureModifier
{
public:
  explicit DoubleAttackModifier(Creature& creature)
    : CreatureModifier(creature)
  {
  }

  void handle() override
  {
    creature.attack *= 2;
    CreatureModifier::handle();
  }
};

class IncreaseDefenseModifier : public CreatureModifier
{
public:
  explicit IncreaseDefenseModifier(Creature& creature)
    : CreatureModifier(creature)
  {
  }


  void handle() override
  {
    if (creature.attack <= 2)
      creature.defense += 1;
    CreatureModifier::handle();
  }
};

int main_()
{
  Creature goblin{ "Goblin", 1, 1 };
  CreatureModifier root{ goblin };
  DoubleAttackModifier r1{ goblin };
  DoubleAttackModifier r1_2{ goblin };
  IncreaseDefenseModifier r2{ goblin };
  //NoBonusesModifier nb{ goblin }; // effectively Command objects

  //root.add(&nb);
  root.add(&r1);
  root.add(&r1_2);
  root.add(&r2);

  root.handle(); // annoying

  cout << goblin << endl;

  //getchar();
  return 0;
}

```

2. Broker Chain (Too Difficult for me)
```cpp

struct Query
{
  string creature_name;
  enum Argument { attack, defense } argument;
  int result;


  Query(const string& creature_name, const Argument argument, const int result)
    : creature_name(creature_name),
      argument(argument),
      result(result)
  {
  }
};

struct Game // mediator
{
  signal<void(Query&)> queries;
};

class Creature
{
  Game& game;
  int attack, defense;
public:
  string name;
  Creature(Game& game, const string& name, const int attack, const int defense)
    : game(game),
      attack(attack),
      defense(defense),
      name(name)
  {
  }
  
  // no need for this to be virtual
  int GetAttack() const
  {
    Query q{ name, Query::Argument::attack, attack };
    game.queries(q);
    return q.result;
  }

  friend ostream& operator<<(ostream& os, const Creature& obj)
  {
    return os
      << "name: " << obj.name
      << " attack: " << obj.GetAttack() // note here
      << " defense: " << obj.defense;
  }
};

class CreatureModifier
{
  Game& game;
  Creature& creature;
public:
  virtual ~CreatureModifier() = default;

  // there is no handle() function

  CreatureModifier(Game& game, Creature& creature)
    : game(game),
      creature(creature)
  {
  }
};

class DoubleAttackModifier : public CreatureModifier
{
  connection conn;
public:
  DoubleAttackModifier(Game& game, Creature& creature)
    : CreatureModifier(game, creature)
  {
    // whenever someone wants this creature's attack,
    // we return DOUBLE the value
    conn = game.queries.connect([&](Query& q)
    {
      if (q.creature_name == creature.name && 
        q.argument == Query::Argument::attack)
        q.result *= 2;
    });
  }

  ~DoubleAttackModifier()
  {
    conn.disconnect();
  }
};

// similar idea, but Query instead of Command
int main(int ac, char* av)
{
  Game game;
  Creature goblin{ game, "Strong Goblin", 2, 2 };

  cout << goblin << endl;

  {
    DoubleAttackModifier dam{ game, goblin };

    cout << goblin << endl;
  }

  cout << goblin << endl;

  getchar();
  return 0;
}

```