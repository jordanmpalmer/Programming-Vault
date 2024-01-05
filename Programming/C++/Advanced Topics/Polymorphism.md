
Why did we have to use Pointers to send subclasses through functions of class type
## What is Polymorphism? 
- Fundamental to Object-Oriented Programming
- Polymorphism
	- Compile-time / early binding / static binding
	- Run-time / late binding / dynamic binding
- Runtime polymorphism
	- Being able to assign different meaning to the same function at run-time
- Allows us to program more abstractly
	- Think general vs. specific
	- Let C++ figure out which function to call at run-time
- Not the default in C++, run-time polymorphism is achieved via 
	- Inheritance
	- Base class pointers or references
	- Virtual functions
## Type of Polymorphism
- Compile-time
	- Function Overloading
	- Operator Overloading
- Run-time
	- Function overriding

Static Binding
```cpp
// These are determined during compile
Account a;
a.withdraw(1000);     // Account::withdraw()

Account b;
b.withdraw(1000);     // Savings::withdraw()

Account c;
c.withdraw(1000);     // Checking::withdraw()

Account d;
d.withdraw(1000);     // Trust::withdraw()


Account* p = new Trust();
p->withdraw(1000);          // Account::withdraw()
                            // We want Trust::withdraw()

void display_account(const Account &acc) {
	acc.display();
	// will always use Account::display even if a trust, savings, or checking account is given
}
```

Dynamic Binding
```cpp
// These are still static bound
Account a;
a.withdraw(1000);     // Account::withdraw()

Account b;
b.withdraw(1000);     // Savings::withdraw()

// withdraw method is virtual in Account
Account* p = new Trust();
p->withdraw(1000);          // Trust::withdraw()

void display_account(const Account &acc) {
	acc.display();
	// will always call the display method depending on 
	// the object's type at RUN-TIME!
}
```

## Using a Base Class Pointer

- For dynamic polymorphism we must have
	- Inheritance
	- Base class pointer or Base class reference
	- virtual functions

```cpp
Account* p1 = new Account();
Account* p2 = new Savings();
Account* p3 = new Checking();
Account* p4 = new Trust();

p1->withdraw(1000);       //Account::withdraw
p2->withdraw(1000);       //Account::withdraw
p3->withdraw(1000);       //Account::withdraw
p4->withdraw(1000);       //Account::withdraw

// delete the pointers
```

```cpp
Account* p1 = new Account();
Account* p2 = new Savings();
Account* p3 = new Checking();
Account* p4 = new Trust();

Account* array [] = {p1,p2,p3,p4};

for (auto i=0; i<4; ++i)
	array[i]->withdraw(1000);

// delete the pointers
```

```cpp
Account* p1 = new Account();
Account* p2 = new Savings();
Account* p3 = new Checking();
Account* p4 = new Trust();

vector<Account*> = {p1,p2,p3,p4};

for (auto acc_ptr: accounts)
	acc_ptr->withdraw(1000);

// delete the pointers
```

## Virtual Functions
- Redefined functions are bound statically 
- Overridden functions are bound dynamically 
- Virtual functions are overridden 
- Allows us to treat all objects generally as objects of the Base class
Declaring virtual functions
- Declare the function you want to override as virtual in the Base class
- Virtual functions are virtual all the way down the hierarchy from this point
- Dynamic polymorphism only via Account class pointer or reference

```cpp
class Account
{
public:
	virtual void withdraw(double amount);
	. . .
};
```

- Overriding the function in the Derived classes
- Function signature and return type must match EXACTLY 
- Virtual keyword not required but is best practice
- If you don't provide an overridden version it is inherited from it's base class
```cpp
class Checking: public Account 
{
public:
	virtual void withdraw(double amount);
	. . . 
};
```

## Virtual Destructors 

- Problems can happen when we destroy polymorphic objects
- If a derived class is destroyed by deleting its storage via the base class pointer and the class a non-virtual destructor. Then the behavior is undefined in the C++ standard. 
- Derived objects must be destroyed in the correct order starting at the correct destructor

Solution/Rule
- If a class has virtual functions
- ALWAYS provide a public virtual destructor 
- If base class destructor is virtual then all derived class destructors are also virtual 

```cpp
class Account
{
public:
	virtual void withdraw(double amount);
	virtual ~Account();
	. . .
};
```

## Using the Override Specifier

- We can override Base class virtual functions
- The function signature and return bust be EXACTLY the same
- If they are different then we have redefinition NOT overriding
- Redefinition is statically bound
- Overriding is dynamically bound
- C++11 provides an override specifier to have the compiler ensure overriding

Error
```cpp
class Base 
{
public:
	virtual void say_hello() const 
	{
		std::cout << "Hello - I'm a Base class object" << std::endl;
	}
	virtual ~Base() {}
};

class Derived: public Base
{
public:
	virtual void say_hello() // Notice I forgot the const - NOT OVERRIDING but function def
	{
		std::cout << "Hello - I'm a Derived class object" << std::endl;
	}
	virtual ~Derived() {}
};


Base* p1 = new Base();
p1->say_hello();          // "Hello - I'm a Base class object"

Base* p2 = new Derived();
p2->say_hello();          // "Hello - I'm a Base class object"
```

Corrected
```cpp
class Base 
{
public:
	virtual void say_hello() const 
	{
		std::cout << "Hello - I'm a Base class object" << std::endl;
	}
	virtual ~Base() {}
};

class Derived: public Base
{
public:
	virtual void say_hello() const override
	{
		std::cout << "Hello - I'm a Derived class object" << std::endl;
	}
	virtual ~Derived() {}
};
```

## Final Specifier

- When used at the class level
	- Prevents a class from being derived from
```cpp
class My_class final 
{
	. . .
};

class Derived final: public Base
{
	. . . 
};
```

- When used at the method level
	- Prevents virtual method from being overridden in derived classes
```cpp
class A 
{
	virtual void do_something();
};

class B: public A 
{
	virtual void do_something() final;  // prevent further overriding
};

class C: public B 
{
	virtual void do_something();   // Compiler error - can't override
};

```

## Using Base Class References

We can also use Base class references with dynamic polymorphism
- Useful when we pass objects to functions that expect a Base class reference

```cpp
Account a;
Account &ref = a;
ref.withdraw(1000); // Account::withdraw

Trust t;
Account &ref1 = t
ref1.withdraw(1000); // Trust::withdraw
```

```cpp
void do_withdraw(Account &account, double amount)
{
	account.withdraw(amount);
}

Account a;
do_withdraw(a, 1000);  // Account::withdraw

Trust t;
do_withdraw(t, 1000);  // Trust::withdraw
```

## Pure Virtual Functions and Abstract Classes

Abstract classes
- Cannot instantiate objects
- These classes are used as base classes in inheritance hierarchies
- Often referred to as Abstract Base Classes
Concrete class
- Used to instantiate objects from
- All their member functions are defined
	- Checking account, savings account
	- Faculty, staff
	- Enemy, level boss

Abstract base class
- Too generic to create objects from
	- Shape, Employee, Account, Player
- Serves as parent to Derived classes that may have objects
- Contains at least one pure virtual function

Pure virtual function
- Used to make a class abstract
- Specified with "=0" in its declaration
- `virtual void function() = 0;  // pure virtual function`
- Typically do not provide implementations

- Derived classes MUST override the base class
- If the Derived class does not override then the Derived class is also abstract
- Used when it doesn't make sense for a base class to have an implementation
	- but concrete classes must implement it

```cpp
virtual void draw() = 0;      // Shape
virtual void defend() = 0;    // Player
```

```cpp
class Shape    // Abstract
{
private: 
	// attributes common to all shapes
public:
	virtual void draw() = 0;   // pure virtual function
	virtual void rotate() = 0; // pure virtual function
	virtual ~Shape();
	. . . 
};

class Circle: public Shape    // Concrete
{
private:     
	// attributes for a circle
public:
	virtual void draw() override
	{
		// code to draw a circle
	}
	virtual void rotate() override
	{
		// code to rotate a circle
	}
	virtual ~Shape();
	. . . 
};
```

Abstract Base class 
- Cannot be instantiated directly 
```cpp
Shape shape;               // error
Shape* ptr = new Shape();  // error
```
- We can use pointers and references to dynamically refer to concrete classes derived from them
```cpp
Shape* ptr = new Circle();
ptr->draw();
ptr->rotate();
```

## Abstract Classes as an Interface

- An abstract class that has only pure virtual functions
- These functions provide a general set of services to the user of the class
- Provided as public
- Each subclass is free to implement these services as needed
- Every service (method) must be implemented
- The services type information is strictly enforced

- C++ does not provide true interfaces
- We use abstract classes and pure virtual functions to achieve it
- Suppose we want to be able to provide Printable support for any object we wish without knowing it's implementation at compile time

- Friend functions are not inherited 

```cpp
std::cout << any_object << std::endl;
// any_object must conform to the Printable interface
```

```cpp
class Printable 
{
	friend std::ostream &operator<<(std::ostream &, const Printable &obj);
public:
	virtual void print(ostream &os) const = 0;
	virtual ~Printable() {};
	. . .
};

std::ostream &operator<<(std::ostream &os, const Printable &obj) 
{
	obj.print(os);
	return os;
}

class Any_Class : public Printable 
{
public:
	// must override Printable::print()
	virtual void print(std::  ostream &os) override 
	{
		os << "Hi from Any_Class"; 
	}
	. . .
};

Any_Class* ptr = new Any_Class();
cout << *ptr << endl;

void function1 (Any_Class &obj)
{
	cout << obj << endl;
}

void function2 (Printable &obj)
{
	cout << obj << endl;
}
function1(*ptr);       // "Hi from Any_Class"
function2(*ptr);       // "Hi from Any_Class"
```