## Pass by Value

Copies value into new variable
```cpp
#include <iostream>

void printValue(int y)
{
    std::cout << y << '\n';
} // y is destroyed here

int main()
{
    int x { 2 };

    printValue(x); // x is passed by value (copied) into parameter y (inexpensive)

    return 0;
}
```


## Pass by lvalue Reference

Avoids expensive copy, any change to y changes the original x
```cpp
#include <iostream>
#include <string>

void printValue(std::string& y) // type changed to std::string&
{
    std::cout << y << '\n';
} // y is destroyed here

int main()
{
    std::string x { "Hello, world!" };

    printValue(x); // x is now passed by reference into reference parameter y (inexpensive)

    return 0;
}
```

## Pass by Const lvalue Reference

R values can be applied to lvalues if they are const. The 5 is a temporary object that will be destroyed when y is.  Imagine `const int& y = 5`.
```cpp
#include <iostream>

void printValue(const int& y) // y is now a const reference
{
    std::cout << y << '\n';
}

int main()
{
    int x { 5 };
    printValue(x); // ok: x is a modifiable lvalue

    const int z { 5 };
    printValue(z); // ok: z is a non-modifiable lvalue

    printValue(5); // ok: 5 is a literal rvalue

    return 0;
}
```

## [Pass by Address](https://www.learncpp.com/cpp-tutorial/pass-by-address/)

Wise to check if the pointer is null in the function, prefer pass by reference most of the time

```cpp
#include <iostream>
#include <string>

void printByAddress(const std::string* ptr) // The function parameter is a pointer that holds the address of str
{
    std::cout << *ptr << '\n'; // print the value via the dereferenced pointer
}

int main()
{
    std::string str{ "Hello, world!" };
    
    printByAddress(&str); // pass str by address, does not make a copy of str

    std::string* ptr { &str }; // define a pointer variable holding the address of str
    printByAddress(ptr); // pass str by address, does not make a copy of str

    return 0;
}
```

#### Common usage 

One of the more common uses for pass by address is to allow a function to accept an “optional” argument. This can be done with overloading though. 
```cpp
#include <iostream>
#include <string>

void greet(std::string* name=nullptr)
{
    std::cout << "Hello ";
    std::cout << (name ? *name : "guest") << '\n';
}

int main()
{
    greet(); // we don't know who the user is yet

    std::string joe{ "Joe" };
    greet(&joe); // we know the user is joe

    return 0;
}
```
This example prints:
>Hello guest
>Hello Joe

## Pass by address by reference
```cpp
#include <iostream>

void nullify(int*& refptr) // refptr is now a reference to a pointer
{
    refptr = nullptr; // Make the function parameter a null pointer
}

int main()
{
    int x{ 5 };
    int* ptr{ &x }; // ptr points to x

    std::cout << "ptr is " << (ptr ? "non-null\n" : "null\n");

    nullify(ptr);

    std::cout << "ptr is " << (ptr ? "non-null\n" : "null\n");
    return 0;
}
```
This program prints:
>ptr is non-null
>ptr is null

> [!NOTE] 
> As an aside…
>Because references to pointers are fairly uncommon, it can be easy to mix up the syntax (is it `int*&` or `int&*`?). The good news is that if you do it backwards, the compiler will error because you can’t have a pointer to a reference (because pointers must hold the address of an object, and references aren’t objects). Then you can switch it around.


## All three

```cpp
#include <iostream>
#include <string>

void printByValue(std::string val) // The function parameter is a copy of str
{
    std::cout << val << '\n'; // print the value via the copy
}

void printByReference(const std::string& ref) // The function parameter is a reference that binds to str
{
    std::cout << ref << '\n'; // print the value via the reference
}

void printByAddress(const std::string* ptr) // The function parameter is a pointer that holds the address of str
{
    std::cout << *ptr << '\n'; // print the value via the dereferenced pointer
}

int main()
{
    std::string str{ "Hello, world!" };

    printByValue(str); // pass str by value, makes a copy of str
    printByReference(str); // pass str by reference, does not make a copy of str
    printByAddress(&str); // pass str by address, does not make a copy of str

    return 0;
}
```

## Return by reference and return by address

```cpp
#include <iostream>
#include <string>

const std::string& getProgramName() // returns a const reference
{
    static const std::string s_programName { "Calculator" }; // has static duration, destroyed at end of program, keep it consts if static

    return s_programName;
}

int main()
{
    std::cout << "This program is named " << getProgramName();

    return 0;
}
```

Dangling reference
```cpp
#include <iostream>
#include <string>

const std::string& getProgramName()
{
    const std::string programName { "Calculator" }; // now a non-static local variable, destroyed when function ends

    return programName;
}

int main()
{
    std::cout << "This program is named " << getProgramName(); // undefined behavior

    return 0;
}
```

### It’s okay to return reference parameters by reference


```cpp
#include <iostream>
#include <string>

// Takes two std::string objects, returns the one that comes first alphabetically
const std::string& firstAlphabetical(const std::string& a, const std::string& b)
{
	return (a < b) ? a : b; // We can use operator< on std::string to determine which comes first alphabetically
}

int main()
{
	std::string hello { "Hello" };
	std::string world { "World" };

	std::cout << firstAlphabetical(hello, world) << '\n';

	return 0;
}
```

This prints:

>Hello

In the above function, the caller passes in two std::string objects by const reference, and whichever of these strings comes first alphabetically is passed back by const reference. If we had used pass by value and return by value, we would have made up to 3 copies of std::string (one for each parameter, one for the return value). By using pass by reference/return by reference, we can avoid those copies.

## It’s okay to return by const reference an rvalue passed by const reference

When an argument for a const reference parameter is an rvalue, it’s still okay to return that parameter by const reference.

This is because rvalues are not destroyed until the end of the full expression in which they are created.

First, let’s look at this example:

```cpp
#include <iostream>
#include <string>

std::string getHello()
{
    return std::string{"Hello"};
}

int main()
{
    const std::string s{ getHello() };

    std::cout << s;

    return 0;
}
```

In this case, `getHello()` returns a `std::string` by value, which is an rvalue. This rvalue is then used to initialize `s`. After the initialization of `s`, the expression in which the rvalue was created has finished evaluating, and the rvalue is destroyed.

Now let’s take a look at this similar example:

```cpp
#include <iostream>
#include <string>

const std::string& foo(const std::string &s)
{
    return s;
}

std::string getHello()
{
    return std::string{"Hello"};
}

int main()
{
    const std::string s{ foo(getHello()) };

    std::cout << s;

    return 0;
}
```

The only difference in this case is that the rvalue is passed by const reference to `foo()` and then returned by const reference back to the caller before it is used to initialize `s`. Everything else works identically.

We discuss a similar case in lesson [14.6 -- Access functions](https://www.learncpp.com/cpp-tutorial/access-functions/).

### The caller can modify values through the reference

When an argument is passed to a function by non-const reference, the function can use the reference to modify the value of the argument.

Similarly, when a non-const reference is returned from a function, the caller can use the reference to modify the value being returned.

Here’s an illustrative example:

```cpp
#include <iostream>

// takes two integers by non-const reference, and returns the greater by reference
int& max(int& x, int& y)
{
    return (x > y) ? x : y;
}

int main()
{
    int a{ 5 };
    int b{ 6 };

    max(a, b) = 7; // sets the greater of a or b to 7

    std::cout << a << b << '\n';

    return 0;
}
```

In the above program, `max(a, b)` calls the `max()` function with `a` and `b` as arguments. Reference parameter `x` binds to argument `a`, and reference parameter `y` binds to argument `b`. The function then determines which of `x` (`5`) and `y` (`6`) is greater. In this case, that’s `y`, so the function returns `y` (which is still bound to `b`) back to the caller. The caller then assigns the value `7` to this returned reference.

Therefore, the expression `max(a, b) = 7` effectively resolves to `b = 7`.

This prints:
>57

## Return by address

**Return by address** works almost identically to return by reference, except a pointer to an object is returned instead of a reference to an object. Return by address has the same primary caveat as return by reference -- the object being returned by address must outlive the scope of the function returning the address, otherwise the caller will receive a dangling pointer.

The major advantage of return by address over return by reference is that we can have the function return `nullptr` if there is no valid object to return. For example, let’s say we have a list of students that we want to search. If we find the student we are looking for in the list, we can return a pointer to the object representing the matching student. If we don’t find any students matching, we can return `nullptr` to indicate a matching student object was not found.

The major disadvantage of return by address is that the caller has to remember to do a `nullptr` check before dereferencing the return value, otherwise a null pointer dereference may occur and undefined behavior will result. Because of this danger, return by reference should be preferred over return by address unless the ability to return “no object” is needed.

> [!best practice]
> Prefer return by reference over return by address unless the ability to return “no object” (using `nullptr`) is important.

![[ReturnTypesWhenOverloadingOperators.pdf]]