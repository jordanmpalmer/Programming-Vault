## Inheritance
- Process of creating new classes from existing classes
- Reuse mechanism

#### Single Inheritance
- A new class is created from another 'single' class

#### Multiple Inheritance
- A new class is created from two (or more) other classes

#### Terminology 
- Base class (parent class, super class)
	- The class being extended or inherited from
- Derived class (child class, sub class)
	- The class being created fro the base class
	- Will inherit attributes and operation from base class
- "Is-A" relationship
	- Public inheritance
	- Derived classes are sub-types of their base classes
	- Can use a derived class object wherever we use a base class object
- Generalization
	- Combining similar classes into a single, more general class based on common attributes
- Specialization
	- Creating new classes from existing classes proving more specialized attributes or operations
- Inheritance or Class Hierarchies
	- Organization of our inheritance relationships

## Public Inheritance vs. Composition
- Both allow reuse of existing classes
- Public Inheritance
	- "is-a" relationship
		- Employee 'is-a' Person
		- Checking Account 'is-a' Account
		- Circle 'is-a' Shape
- Composition
	- "has-a" relationship
		- Person "has-a" Account
		- Player "has-a" Special Attack
		- Circle "has-a" Location

Composition
```cpp
class Person {
private:
	std::string name;   // has-a name
	Account account;    // has-a account
}
```

```cpp
class Base {
   // Base class member . . .
}

class Derived: access-specifier Base {
   // Derived class members . . . 
}

// Access-specifier can be: public, private, or protectted
```

- Public 
	- Most common
	- Establishes 'is-a' relationship between Derived and Base classes
- Private and protected
	- Establishes "derived class has a base class" relationship
	- "Is implemented in terms of" relationship
	- Different from composition

```cpp
class Account {
   // Account class members . . . 
}

class Savings_Account: public Account {
   // Savings_Account class members . . . 
}

Savings_Account 'is-a' Account

int main () {
	Account account {};
	Account *p_account = new Account();
	
	account.deposit(1000.0);
	p_account->withdraw(200.0);
	
	delete p_account;
	
	
	Savings_Account sav_account {};
	Savings_Account *p_sav_account = new Savings_Account();
	
	sav_account.deposit(1000.0);
	p_sav_account->withdraw(200.0);
	
	delete p_sav_account;
}
```

## Protected Members

```cpp
class Base {
protected: 
	// protected Base class members . . . 
};
```
- Accessible from the Base class itself 
- Accessible from classes Derived from Base
- Not accessible by objects of Base or Derived 

## Constructors and Destructors 

- A Derived class inherits from its Base class
- The Base part of the Derived class MUST be initialized BEFORE the Derived class is initialized
- When a Derived object is created 
	- Base class constructor executes then
	- Derived class constructor executes

- Class destructors are invoked in the reverse order as constructors 
- The Derived part of the Derived class MUST be destroyed BEFORE the Base class destructor is invoked
- When a Derived object is destroyed
	- Derived class destructor executes then
	- Base class destructor executes
	- Each destructor should free resources allocated in it's own constructors

- A Derived class does NOT inherit
	- Base class constructors
	- Base class destructor
	- Base class overloaded assignment operators 
	- Base class friend functions
- However, the derived class constructors, destructors, and overloaded assignment operators can invoke the base-class version

## Passing Arguments to Base Class Constructor

- The Base part of a Derived class must be initialized first
- How can we control exactly which Base class constructor is used during initialization? 
	- We can invoke the whichever Base class constructor we wish in the initialization list of the Derived class

```cpp
class Base 
{
public:
	Base();
	Base(int);
	. . . 
};

Derived::Derived(int x)
	: Base(x), {//optional initializers for Derived} 
{
	// code
}
```

## Copy/Move Constructors and Overloaded Operator=

- Not inherited from the Base class
- You may not need to provide your own
	- Compiler-provided versions may be fine
- We can explicitly invoke the Base class versions from the Derived class
	- Derived object 'other' will be sliced

```cpp
Derived::Derived(const Derived &other)
	: Base(other), {//Derived initialization list}
{
	// code
}

// Base expects a base object but is getting a Derived objects. This triggers the program to be sliced, slicing out the base part.
```

operator= 
```cpp
class Derived : public Base 
{
	int doubled_value;
public:
	// same constructors as previous example
	Derived &operator=(const Derived &rhs) 
	{
		if (this != &rhs)
		{
			Base::operator=(rhs);              // Assign Base part
			doubled_value = rhs.doubled_value; // Assign Derived part
		}
		return *this;
	}
};
```

## Using and Redefining Base Class Methods

- Derived class can directly invoke Base class methods
- Derived class can override or redefine Base class methods
- Very powerful in the context of polymorphism

```cpp
class Account 
{
public: 
	void deposit(double amount) {balance += amount;}
}

class Savings_Account: public Account 
{
public:
	void deposit(double amount)
	{
	// Redefine Base class method
	amount += some_interest;
	Account::deposit(amount); // invoke call Base class method
	}
}
```

## Static Binding of Method Calls

- Binding of which method to use is done at compile time
	- Default binding for C++ is static
	- Derived class objects will use Derived:deposit
	- But, we can explicitly invoke Base::deposit from Derived::deposit
	- Ok, but limited - much more powerful approach is dynamic binding

You can do
```cpp
Base *ptr = new Derived();
ptr->deposit(1000.0); // does it use Base::deposit or Derived::deposit?

// It will use the Base::deposit
```

## Multiple Inheritance

- A derived class inherits fro two or more Base classes at the same time
- The Base classes may belong to unrelated class hierarchies
- A Department Chair
	- Is-A Faculty and
	- Is-A Administrator

```cpp
class Department_Chair: 
	public Faculty, public Administrator
{
   . . . 
};
```

