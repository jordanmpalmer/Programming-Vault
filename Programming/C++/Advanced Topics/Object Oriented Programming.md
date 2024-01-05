### Declaring a Class

```cpp
class Player 
{
public:
    // attributes
    std::string name; 
    int health; 
    int xp;

    //methods
    void talk(std::string text_to_say);
    bool is_dead();
};

int main() {
    Player frank;
    Player hero;

    Player players[] {frank, hero};

    std::vector<Player> players1 {frank};
    players1.push_back(hero);

    Player *enemy = new Player();
    delete enemy;
    
    return 0;
}
```

### Access Class Attributes and Methods

```cpp
    frank_account.balance;
    frank_account.deposit(1000.00);

    frank.name = "Frank";
    frank.health = 100;
    frank.xp = 12;

	// pointers
	// dereference pointer
    (*frank_account).balance;
    (*frank_account).deposit(1000.00);
	// pointer syntax
    frank_account->balance;
    frank_account->deposit(1000.00);
```

### Public, Private, and Protected

Public
- Accessible anywhere

Private 
- Accessible by members or friends of the class (prevent users from being able to change values like giving a player 10,000 health)

Protected
- used with inheritance 

```cpp
class Player 
{
public:
    std::string name; 
    int health; 
    int xp;

private:
    void talk(std::string text_to_say);
    bool is_dead();
};
```

### Implementing Member Methods

inside class declaration
```cpp
class Account {
private:
    double balance;

public:
    bool set_balance(double bal){
        balance = bal;
    }
    bool get_balance() {
        return balance;
    }
};
```

outside class declaration
```cpp
class Account {
private:
    double balance;

public:
    void set_balance(double bal);
    double get_balance();
};

void Account::set_balance(double bal){
    balance = bal;
}
double Account::get_balance() {
    return balance;
}
```

separate header

```cpp
"Account.h"
// pragma one (does the same thing as the header guard)
#ifndef _ACCOUNT_H_
#define _ACCOUNT_H_

class Account {
private:
    double balance;

public:
    void set_balance(double bal);
    double get_balance();
};
#endif
```

```cpp
"Account.cpp"
#include "Account.h"

void Account::set_balance(double bal){
    balance = bal;
}

double Account::get_balance() {
    return balance;
}
```

```cpp
"main.cpp"
#include <iostream>
#include "Account.h"
int main() {
    Account frank_account;
    frank_account.set_balance(1000.00);
    double bal = frank_account.get_balance();

    std::cout << bal <<std::endl; // 1000
    
    return 0;
}
```

### Constructors and Destructors 

Constructors
- Special member method
- Invoked during object creation
- Useful for initialization
- Same name as the class
- No return type is specified
- Can be overloaded
Destructor 
- Special member method
- Same name as the class proceeded with a tilde (~)
- Invoked automatically when an object is destroyed
- No return type and no parameters
- Only 1 destructor is allowed per class - cannot be overloaded!
- Useful to release memory and other resources

```cpp
class Player 
{
private:
    std::string name; 
    int health; 
    int xp;
public:
    // Overloaded constructors
    Player();
    Player(std::string name);
    Player(std::string name, int health, int xp);
    // Destructor 
    ~Player();
};
```

Default Constructor
- Does not expect any arguments
- If you write no constructors at all for a class, C++ will generate a default constructor to do nothing
- Called when instantiate a new object with no arguments
- If any constructor is defined then no default constructor is automatically made

Overloading Constructors
- Classes can have as many constructors as necessary
- Each must have a unique signature

```cpp
class Player 
{
private:
    std::string name; 
    int health; 
    int xp;
public:
    // Overloaded constructors
    Player();
    Player(std::string name_val);
    Player(std::string name_val, int health_val, int xp_val);
};

Player::Player() {
	name = "None";
	health = 0;
	xp = 0;
}

Player::Player(std::string name_val)) {
	name = name_val;
	health = 0;
	xp = 0;
}

Player::Player(std::string name_val, int health_val, int xp_val){
	name = name_val;
	health = health_val;
	xp = xp_val;
}
```

### Constructor Initialization Lists

- More efficient
- Immediately follows the parameter list
- Initializes the data members as the object is created
- Order of initialization is the order of declaration in the class

```cpp
class Player 
{
private:
    std::string name; 
    int health; 
    int xp;
public:
    Player();
    Player(string name_val);
    Player(string name_val, int health_val, int xp_val);
};

Player::Player()
    :name{"None"},health{0},xp{0} { 
}
Player::Player(string name_val)
    :name{name_val},health{0},xp{0} { 
}
Player::Player(string name_val, int health_val, int xp_val) 
    :name{name_val},health{health_val},xp{xp_val} {
};
```

### Delegating Constructors

```cpp
Player::Player(string name_val, int health_val, int xp_val) 
    :name{name_val},health{health_val},xp{xp_val} {
}
Player::Player()
    : Player {"None", 0, 0} { 
}
Player::Player(string name_val)
    : Player {name_val, 0, 0} { 
}
```

### Default Constructor Parameters

```cpp
class Player 
{
private:
    std::string name; 
    int health; 
    int xp;
public:
    Player(string name_val = "None", 
           int health_val = 0, 
           int xp_val = 0);
};

Player::Player(string name_val, int health_val, int xp_val) 
    :name{name_val},health{health_val},xp{xp_val} {
}
```

### Copy Constructor 

When is a copy of an object made? 
- Passing object by value as a parameter
- Returning an object from a function by value
- Constructing one object based on another of the same class

```cpp
// Pass object by value
Player hero {"Hero", 100, 20};

void display_player(Player p) {
	// p is a COPY of hero in this example
	// use p
	// Destructor for p will be called
}

display_player(hero);


// Return object by value
Player enemy;

Player create_super_enemy() {
	Player an_enemy{"Super Enemy", 1000, 1000};
	return an_enemy;   // A COPY of an_enemy is returned
}

enemy = create_super_enemy();


// Construct one object based on another
Player hero {"Hero", 100, 100};

Player another_hero {hero}; // A COPY of hero is made
```

C++ generates one by default
Will generally be used when copying a pointer
- Can cause shallow vs deep copy

```cpp
class Player 
{
private:
    std::string name; 
    int health; 
    int xp;
public:
    // copy constructor
    Player::Player(const Player &source);
};

Player::Player(const Player &source) 
    : name{source.name},
      health {source.health},
      xp {source.xp} { 
}
```

### Shallow Copy

Default copy constructor 
- Memberwise copy
- Each data member is copied from the source object
- The pointer contents is copied NOT what it points to (shallow copy)
- Problem: when we release the storage in the destructor, the other object still refers to the released storage

Shallow copy 
```cpp
class Shallow {
private:
	int *data                         // Pointer
public:
	Shallow(int d);                   // Constructor
	Shallow(const Shallow &source);   // Copy
	~Shallow();                       // Destructor
}

Shallow::Shallow(int d) {
	data = new int;
	*data = d;
}
Shallow::~Shallow() {
	delete data;       // free storage
	cout << "Destructor freeing data" << endl;
}
Shallow::Shallow(const Shallow &source) 
	: data(source.data) {
		cout << "Copy constructor - shallow" << endl;
}

int main() {
	Shallow obj1 {100};
	display_shallow(obj1);
	// obj1's data has been released!

	obj1.set_data_value(1000);
	Shallow obj2 {obj1};
	cout << "Hello World" << endl;
	return 0;
}
```

### Deep copy
- Create a copy of the pointed-to data
- Each copy will have a pointer to unique storage in the heap
- Deep copy when you have a raw pointer as a class data member

```cpp
class Deep {
private:
	int *data                         // Pointer
public:
	Deep(int d);                   // Constructor
	Deep(const Deep &source);   // Copy
	~Deep();                       // Destructor
}

Deep::Deep(int d) {
	data = new int;
	*data = d;
}
Deep::~Deep() {
	delete data;       // free storage
	cout << "Destructor freeing data" << endl;
}


// Copy constructor
Deep::Deep(const Deep &source) {
	data = new int;       // allocate storage
	*data = *source.data;
	cout << "Copy constructor - deep" << endl;
}
// **OR** Optionally delegate to constructor
Deep::Deep(const Deep &source) 
	:Deep{*source.data}{
	cout << "Copy constructor - deep" << endl;
}


void display_deep(Deep s) {
	cout << s.get_data_value() << endl;
}

int main() {
	Deep obj1 {100};
	display_deep(obj1);
	// obj1's data has been released!

	obj1.set_data_value(1000);
	Deep  obj2 {obj1};
	cout << "Hello World" << endl;
	return 0;
}
```

### Move Constructor 

`total = 100 + 200;`
- 100 + 200 is evaluated and 300 is stored in an unnamed temp value
- The 300 is then stored in the variable total
- Then the temp value is discarded

Copying objects can be expensive so instead of copying the object it will move it

r-value references - references to temporary values
- && is R-value reference operator

```cpp
int x{100};
	int &l_ref = x;       // l-value reference
	l_ref = 10;           // change x to 10

	int &&r_ref = 200;    // r-value ref
	r_ref = 300;          // change r_ref to 300

	int &&x_ref = x;      // compiler error, cannot assign r-val an l-val
```

```cpp
int x {100};         // x is an l-value

void func(int &num); // A

func(x);             // calls A - x is an l-value
func(200);           // Error - 200 is an r-value


int x {100};         // x is an l-value

void func(int &&num); // A

func(x);             // Error - x is an l-value
func(200);           // calls B - 200 is an r-value


// overload
int x {100};         // x is an l-value

void func(int &num); // A
void func(int &&num); // B

func(x);             // calls A - x is an l-value
func(200);           // calls B - 200 is an r-value
```

Moving objects
- Instead of making a deep copy of the move constructor
	- 'moves' the resource
	- Simply copies the address of the resource from source to the current object
	- And, nulls out the pointer in the source pointer
```cpp
class Move {
private:
	int *data;
public:
void set_data_value(int d)     {*data = d;}
int get_data_value()           {return *data;}
Move(int d);                   // Constructor
Move(const Move &source);      // Copy Constructor
Move(Move &&source);           // Move Constructor
~Move();                       // Destructor
};
 
// move constructor
Move::Move (Move &&source) 
	: data{source.data} {
		source.data = nullptr;
}

main() {
	Vector<Move> vec; 

	vec.push_back(Move{10});
	vec.push_back(Move{20});
	return 0;
}
```

### 'this' Pointer

Contains the address of the object, so it's a pointer to the object

```cpp
void Account::set_balance(double bal) {
	balance = bal;      // this->balance is implied
}

void Account::set_balance(double balance) {
	this->balance = bal;      // Unambiguous 
}

int Account::compare_balance(const Account &other) {
	if (this == &other)
		std::cout << "The same objects" << std::endl;
		...
}
frank_account.compare_balance(frank_account);
```

### Creating Const Objects

Cannot use methods or attributes
```cpp
const Player villain {"Villain", 100, 55};

// This will tell the compiler that the method won't change the object
public:
	std::string get_name() const;
```

### Static Class Members

Classic data members can be declared as static
- A single data member that belongs to the class, not the objects
- Useful to store **class-wide information**
Class functions can be declared as static
- Independent of any objects
- Can be called using the class name

```cpp
class Player {
private:
	static int num_players;
public:
	static int get_num_players();
}

"Player.cpp"

#include "Player.h"

int Player::num_players = 0;

int Player::get_num_players() {
	return num_players;
}

Player::Player(std::string name_val, int health_val, int xp_val) 
	: name{name_val}, health{health_val}, xp{xp_val} {
		++num_players;
	}
Player::~Player() {
	--num_player;
}

void display_active_players() {
	cout << "Active players: "
		 << Player::get_num_players() << endl;
}

int main() {

	display_active_players();

	Player obj1 {"Frank"};
	display_active_players();

	return 0;
}
```

### Struct

Members of struct are public by default vs private in a class

```cpp
struct Person {
	std::string name;
	std::string get_name()
};

Person p;
p.name = "Frank";
std::cout << p.get_name();
```

Struct 
- Use struct for passive objects with public access
- Don't declare methods in struct

Class 
- Use class for active objects with private access
- Implement getters/setters as needed
- Implement member methods as needed

### Friends of a Class

Friend 
- A function or class that has access to private class member
- And, that function of or class is NOT a member of the class it is accessing 

Function
- Can be regular non-member functions
- Can be member methods of another class

Class 
- Another class can have access to private class members

```cpp
class Player {
	friend void display_player(Player &p);
	std::string name;
	int health;
	int xp;
public:
};

void display_player(Player &p) {
	std::cout << p.name << std::endl;
	std::cout << p.health << std::endl;
	std::cout << p.xp << std::endl;
}

display_player // may also change private data members


class Player {
	friend void Other_class::display_player(Player &p);
	std::string name;
	int health;
	int xp;
public:
};


class Player {
	friend class Other_class;
	std::string name;
	int health;
	int xp;
public:
};
```
