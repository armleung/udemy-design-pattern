# SOLID Design Principle
- <span style="color:red">S</span>ingle responsibility principle 
- <span style="color:red">O</span>pen/closed principle 
- <span style="color:red">L</span>iskov substitution principle
- <span style="color:red">I</span>nterface segregation principle
- <span style="color:red">D</span>ependency inversion principle

## Open Closed Principle 
> A module should be open for extension but closed for modification.

Skills 
- Dynamic Polymorphism 
```cpp
class Modem
{
    public:
        virtual void Dail(const string& pno) = 0;
        virtual void Send(char) = 0;
        virtual char Recv() = 0;
        virtual void Hangup() = 0;
};
void LogOn(Modem& m, string& pno, string& user, string& pw)
{
    m.Dial(pno);
}
```
- Static Polymorphism (Template)

```cpp
template <typename MODEM>
void LogOn(MODEM& m,string& pno, string& user, string& pw)
{
    m.Dial(pno);
}
```

## The Liskov Substitution Principle (LSP)
> Subclasses should be substitutable for their base classes.

## The Interface Segregation Principle (ISP)
> Many client specific interfaces are better than one general purpose interface

## The Dependency Inversion Principle (DIP)
> Depend upon Abstractions. Do not depend upon concretions.

- Depdending upon Abstractions
- Mitigating Forces
- Object Creation