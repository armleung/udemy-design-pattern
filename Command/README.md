## Command Pattern (Object Behavioral)
![Image](https://refactoring.guru/images/patterns/content/command/command-en.png)

### :octocat: Intent
>Command is a behavioral design pattern that turns a request into a stand-alone object that contains all information about the request. This transformation lets you parameterize methods with different requests, delay or queue a request’s execution, and support undoable operations.

### :octocat: Structure
![Image](https://refactoring.guru/images/patterns/diagrams/command/structure.png)

### :octocat: Applicability 
- Use the Command pattern when you want to parametrize objects with operations.
- Use the Command pattern when you want to queue operations, schedule their execution, or execute them remotely.
- Use the Command pattern when you want to implement reversible operations.

### :octocat: Consequences
- Single Responsibility Principle. You can decouple classes that invoke operations from classes that perform these operations.
- Open/Closed Principle. You can introduce new commands into the app without breaking existing client code.
- You can implement undo/redo.
- You can assemble a set of simple commands into a complex one.

### :octocat: Related Patterns
- [Chain of Responsibility](https://github.com/armleung/udemy-design-pattern/tree/master/Chain%20of%20Responsibility)
    - **Chain of Responsibility** passes a request sequentially along a dynamic chain of potential receivers until one of them handles it.
    - **Command** establishes unidirectional connections between senders and receivers.
    - Handlers in **Chain of Responsibility** can be implemented as **Commands**. In this case, you can execute a lot of different operations over the same context object, represented by a request.
- [Prototype](https://github.com/armleung/udemy-design-pattern/tree/master/Prototype)
    - **Prototype** can help when you need to save copies of **Commands** into history.

### :octocat: Implementation
1. undoable
```cpp

struct BankAccount
{ 
  int balance = 0;
  int overdraft_limit = -500;

  void deposit(int amount)
  {
    balance += amount;
    cout << "deposited " << amount << ", balance now " << 
      balance << "\n";
  }

  void withdraw(int amount)
  {
    if (balance - amount >= overdraft_limit)
    {
      balance -= amount;
      cout << "withdrew " << amount << ", balance now " << 
        balance << "\n";
    }
  }
};

struct Command
{
  virtual ~Command() = default;
  virtual void call() const = 0;
  virtual void undo() const = 0;
};

// should really be BankAccountCommand
struct BankAccountCommand : Command
{
  BankAccount& account;
  enum Action { deposit, withdraw } action;
  int amount;

  BankAccountCommand(BankAccount& account, 
    const Action action, const int amount)
    : account(account), action(action), amount(amount) {}

  void call() const override
  {
    switch (action)
    {
    case deposit:
      account.deposit(amount);
      break;
    case withdraw: 
      account.withdraw(amount);
      break;
    default: break;
    }
  }

  void undo() const override
  {
    switch (action)
    {
    case withdraw:
      account.deposit(amount);
      break;
    case deposit:
      account.withdraw(amount);
      break;
    default: break;
    }
  }
};
```