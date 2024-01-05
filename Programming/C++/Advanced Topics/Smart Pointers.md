## Issues with Raw Pointers
- C++ provides absolute flexibility with memory management
	- Allocation
	- Deallocation
	- Lifetime management
- Some potentially serious problems
	- Uninitialized (wild) pointers
	- Memory leaks
	- Dangling pointers
	- Not exception safe
- Ownership ?
	- Who owns the pointer? 
	- When should a pointer be deleted?

## What is a Smart Pointer?

- Objects
- Can only point to heap-allocated memory
- Automatically call delete when no longer needed
- Adhere to RAII principles
- C++ Smart pointers
	- Unique pointers (unique_ptr)
	- Shared pointers (shared_ptr)
	- Weak pointers (weak_ptr)
	- Auto pointers (auto_ptr) // DEPRECATED

- \#include <\memory>
- Defined by class templates
	- Wrapper around a raw pointer
	- Overloaded operators
		- Dereference (\*)
		- Member selection (->)
		- Pointer arithmetic not supported (++,--,etc.)
	- Can have custom deleters

```cpp
{
std::smart_pointer<Some_Class> ptr = . . . 

ptr->method();
cout << (*ptr) << endl;
}

// ptr will be destroyed automatically when no longer needed
```

## Resource Acquisition Is Initialization (RAII)

- Common idiom or pattern used in software design based on container object lifetime
- RAII objects are allocated on the stack
- Resource Acquisition
	- Open a file
	- Allocate memory
	- Acquire a lock
- Is Initialization
	- The resource is acquired in a constructor
- Resource relinquishing 
	- Happens in the destructor 
		- Close the file
		- Deallocate the memory
		- Release the lock

## Unique Pointers

- Simple s,art pointer - very efficient!
- unique_ptr<\T>
	- Points to an object of type T on the heap
	- It is unique - there can only be one unique_ptr<\T> pointer to the object on the heap
	- Owns what it points to
	- Cannot be assigned or copied
	- CAN be moved
	- When the pointer is destroyed, what it points to is automatically destroyed

```cpp
{
	std::unique_ptr<int> p1 {new int {100}};
	std::cout << *p1 << std::endl;   // 100
	*p1 = 200;
	std::cout << *p1 << std::endl;   // 200
	
} // automatically deleted
```

```cpp
{
	std::unique_ptr<int> p1 {new int {100}};
	std::cout << p1.get() << std::endl;   // 0x564388
	p1.reset(); // p1 is now a nullptr
	
	if(p1)
		std::cout << *p1 << std::endl;   // won't execute
} // automatically deleted
```

```cpp
{
	std::unique_ptr<Account> p1 {new Account{"Larry"}};
	std::cout << *p1 << std::endl;  // display account
	p1->deposit(1000);
	p1->withdraw(500);
}// automatically deleted
```

can't be copied or assigned
```cpp
{
	std::vector<std::unique_ptr<int>> vec;
	std::unique_ptr<int> ptr {new int{100}};
	
	vec.push_back(ptr);   // Error - copy not allowed
	vec.push_back(std::move(ptr));
}// automatically deleted
```

Best way to make unique_ptrs
```cpp
{
	std::unique_ptr<int> p1 = make_unique<int>(100);
	std::unique_ptr<Account> p2 = make_unique<Account>("Curly", 5000);
	auto p3 = make_unique<Player>("Hero", 100, 100);
}// automatically deleted
```

## Shared Pointer
- Provides shared ownership of heap properties
- shared_ptr<\T>
	- Points to an objects of type T on the heap
	- It is not unique - there can be many shared_ptrs pointing to the same object on the heap
	- Establishes shared ownership relationship
	- CAN be assigned and copied
	- CAN be moved
	- Doesn't support managing arrays by default
	- When the use count is zero, the managed object on the heap is destroyed

```cpp
{
	std::shared_ptr<int> p1 {new int {100} };
	std::cout << *p1 << std::endl; // 100
	
	*p1 = 200;

	std::cout << *p1 << std::endl; // 200
}
```

```cpp
{
	// use_count - the number of shared_ptr objects managing the heap object
	std::shared_ptr<int> p1 {new int {100} };
	std::cout << p1.use_count() << std::endl;  // 1
	
	std::shared_ptr<int> p2 { p1 };            // shared ownership
	std::cout << p1.use_count() << std::endl;  // 2
	
	p1.reset();    // decrement the use_cout; p1 is nulled out
	std::cout << p1.use_count() << std::endl;  // 0
	std::cout << p2.use_count() << std::endl;  // 1
} // automatically deleted
```

can be copy, assigned, and moved
```cpp
{
	std::vector<std::shared_ptr<int>> vec;
	
	std::shared_ptr<int> ptr {new int{100}};
	
	vec.push_back(ptr);  // OK - copy IS allowed
	
	std::cout << ptr.use_count() << std::endl;   // 2
} // automatically deleted
```

```cpp
{
	std::shared_ptr<int> p1 = make_shared<int>(100); // use_count: 1
	std::shared_ptr<int> p2 {p1};      // use_count: 2
	std::shared_ptr<int> p3; 
	p3 = p1;                           // use_count: 3
}// automatically deleted
```

## Weak Pointer

- Provides a non-owning "weak" reference
- weak_ptr<\T>
	- Points to an object of type T on the heap
	- Does not participate in owning relationship
	- Always created from a shared_ptr
	- Does NOT increment or decrement reference use count
	- Used to prevent strong reference cycles which could prevent objects from being deleted

## Custom Deleters 
- Sometimes when we destroy a smart pointer we need more than to just destroy the object on the heap
- There are special use-cases
- C++ smart pointers allow you to provide custom deleters
- Lots of ways to achieve this
	- Functions
	- Lambdas
	- Others. . .

```cpp
void my_deleter(Some_Class *raw_pointer) 
{
	// your custom deleter code
	delete raw_pointer;
}

// when creating pointer, also pass deleter
shared_ptr<Some_Class> ptr { new Some_class{}, my_deleter };


// for a lambda
std::shared_ptr<Test> ptr2 (new Test{100}, [] (Test* ptr) {
	std::cout << "\tUsing my custom lambda deleter" << std::endl;
	delete ptr;
});
```


 

