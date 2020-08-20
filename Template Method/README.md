## Template Method Pattern(Class Behavioral)
![Image](https://refactoring.guru/images/patterns/content/template-method/template-method.png)

### :octocat: Intent
> Template Method is a behavioral design pattern that defines the skeleton of an algorithm in the superclass but lets subclasses override specific steps of the algorithm without changing its structure.

### :octocat: Structure
![Image](https://refactoring.guru/images/patterns/diagrams/template-method/structure.png)

### :octocat: Applicability 
- The Template Method lets you turn a monolithic algorithm into a series of individual steps which can be easily extended by subclasses while keeping intact the structure defined in a superclass.
- avoid code duplication

### :octocat: Consequences
Template methods call the following kinds of operations
- concrete operations
- concrete AbstractClass operations
- primitive operations
- factory methods
- hook operations

### :octocat: Related Patterns
- [Factory Method](https://github.com/armleung/udemy-design-pattern/tree/master/Factory%20Method)
    - **Factory Method** is a specialization of **Template Method**. At the same time, a **Factory Method** may serve as a step in a large **Template Method**.
- [Stragegy](https://github.com/armleung/udemy-design-pattern/tree/master/Stragegy)
    - **Template Method** is based on inheritance: it lets you alter parts of an algorithm by extending those parts in subclasses. **Strategy** is based on composition: you can alter parts of the object’s behavior by supplying it with different strategies that correspond to that behavior. **Template Method** works at the class level, so it’s static. Strategy works on the object level, letting you switch behaviors at runtime.

### :octocat: Implementation
```cpp
class Game
{
public:
	explicit Game(int number_of_players)
		: number_of_players(number_of_players)
	{
	}

	void run()
	{
		start();
		while (!have_winner())
			take_turn();
		cout << "Player " << get_winner() << " wins.\n";
	}

protected:
	virtual void start() = 0;
	virtual bool have_winner() = 0;
	virtual void take_turn() = 0;
	virtual int get_winner() = 0;

	int current_player{ 0 };
	int number_of_players;
};

class Chess : public Game
{
public:
	explicit Chess() : Game{ 2 } {}

protected:
	void start() override
	{
		cout << "Starting a game of chess with " << number_of_players << " players\n";
	}

	bool have_winner() override
	{
		return turns == max_turns;
	}

	void take_turn() override
	{
		cout << "Turn " << turns << " taken by player " << current_player << "\n";
		turns++;
		current_player = (current_player + 1) % number_of_players;
	}

	int get_winner() override
	{
		return current_player;
	}

private:
	int turns{ 0 }, max_turns{ 10 };
};

int main()
{
	Chess chess;
	chess.run();

	getchar();
	return 0;
}
```