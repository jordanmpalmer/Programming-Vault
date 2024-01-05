## Declaring a const variable

To declare a constant variable, we place the `const` keyword (called a “const qualifier”) adjacent to the object’s type:

```cpp
const double gravity { 9.8 };  // preferred use of const before type
int const sidesInSquare { 4 }; // "east const" style, okay but not preferred
```

Although C++ will accept the const qualifier either before or after the type, it’s much more common to use `const` before the type because it better follows standard English language convention where modifiers come before the object being modified (e.g. a “a green ball”, not a “a ball green”).

### Const variables must be initialized

Const variables _must_ be initialized when you define them, and then that value can not be changed via assignment:

```cpp
int main()
{
    const double gravity; // error: const variables must be initialized
    gravity = 9.9;        // error: const variables can not be changed

    return 0;
}
```

## Naming your const variables

There are a number of different naming conventions that are used for const variables.

Programmers who have transitioned from C often prefer underscored, upper-case names for const variables (e.g. `EARTH_GRAVITY`). More common in C++ is to use intercapped names with a ‘k’ prefix (e.g. `kEarthGravity`).

However, because const variables act like normal variables (except they can not be assigned to), there is no reason that they need a special naming convention. For this reason, we prefer using the same naming convention that we use for non-const variables (e.g. `earthGravity`).

## Const return values

A function’s return value may also be made const:

```cpp
#include <iostream>

const int getValue()
{
    return 5;
}

int main()
{
    std::cout << getValue() << '\n';

    return 0;
}
```

For fundamental types, the `const` qualifier on a return type is simply ignored (your compiler may generate a warning).

For other types (which we’ll cover later), there is typically little point in returning const objects by value, because they are temporary copies that will be destroyed anyway. Returning a const value can also impede certain kinds of compiler optimizations (involving move semantics), which can result in lower performance.

> [!NOTE]
> Best practice
> Don’t use `const` when returning by value.

## Object-like macros with substitution text

In lesson [2.10 -- Introduction to the preprocessor](https://www.learncpp.com/cpp-tutorial/introduction-to-the-preprocessor/), we discussed object-like macros with substitution text. For example:

```cpp
#include <iostream>

#define MY_NAME "Alex"

int main()
{
    std::cout << "My name is: " << MY_NAME << '\n';

    return 0;
}
```

When the preprocessor processes the file containing this code, it will replace `MY_NAME` (on line 7) with `"Alex"`. Note that `MY_NAME` is a name, and the substitution text is a constant value, so object-like macros with substitution text are also named constants.

## Prefer constant variables to preprocessor macros

So why not use preprocessor macros for named constants? There are (at least) three major problems.

The biggest issue is that macros don’t follow normal C++ scoping rules. Once a macro is #defined, all subsequent occurrences of the macro’s name in the current file will be replaced. If that name is used elsewhere, you’ll get macro substitution where you didn’t want it. This will most likely lead to strange compilation errors. For example:

```cpp
#include <iostream>

void someFcn()
{
// Even though gravity is defined inside this function
// the preprocessor will replace all subsequent occurrences of gravity in the rest of the file
#define gravity 9.8
}

void printGravity(double gravity) // including this one, causing a compilation error
{
    std::cout << "gravity: " << gravity << '\n';
}

int main()
{
    printGravity(3.71);

    return 0;
}
```

When compiled, GCC produced this confusing error:

prog.cc:5:17: error: expected ',' or '...' before numeric constant
    5 | #define gravity 9.8
      |                 ^~~
prog.cc:9:26: note: in expansion of macro 'gravity'

Second, it is often harder to debug code using macros. Although your source code will have the macro’s name, the compiler and debugger never see the macro because it has already been replaced before they run. Many debuggers are unable to inspect a macro’s value, and often have limited capabilities when working with macros.

Third, macros substitution behaves differently than everything else in C++. Inadvertent mistakes can be easily made as a result.

Constant variables have none of these problems: they follow normal scoping rules, can be seen by the compiler and debugger, and behave consistently.

Best practice

Prefer constant variables over object-like macros with substitution text.

### Const Variables
```cpp
int main() {

	// i cannot be changed
	const int i{0};

	// value of what it is pointing at cannot change
	const int* ptr = new int;

	// address of pointer cannot change
	int* const ptr = new int;
	
	
	return 0;
}
```

### Const Classes and Functions
```cpp
class Entity {
private:
	int m_X, m_Y;   (int* m_X, m_Y //m_Y is not a pointer, need int* m_X, *m_Y  )
	int *m_Z;
public:
	// this method won't modify class, cannot call PrintEntity if it doesn't guaranteee const
	int getX() const {
		return m_X;
	}
	// often duplicate functions to allow returning no const
	int getX() {
		return m_X;
	}
	// returns constant pointer, with contents that can't be modified, in a function that won't change the value
	const int* const getZ const {
		return m_Z;
	}
};

void PrintEntity(const Entity& e) {
	std::cout << e.getX() << std::endl;
}

int main() {

	Entity e;
	PrintEntity(e);
	
	return 0;
}
```

### Mutable 
```cpp
class Entity {
private:
	// mutable allows const to be changed
	mutable int var;
public:
	int getX() const {
		var = 2;
		return m_X;
	}
};
```

## Compile-time const

A const variable is a compile-time constant if its initializer is a constant expression.

Consider a program similar to the above that uses const variables:

```cpp
#include <iostream>

int main()
{
	const int x { 3 };  // x is a compile-time const
	const int y { 4 };  // y is a compile-time const

	const int z { x + y }; // x + y is a constant expression, so z is compile-time const

	std::cout << z << '\n';

	return 0;
}
```

## Runtime const

A const variable is a runtime constant if its initializer is a non-constant expression. **Runtime constants** are constants whose initialization values can’t be determined until runtime.

The following example illustrates the use of a constant that is a runtime constant:

```cpp
#include <iostream>

int getNumber()
{
    std::cout << "Enter a number: ";
    int y{};
    std::cin >> y;

    return y;
}

int main()
{
    const int x { 3 };           // x is a compile time constant

    const int y { getNumber() }; // y is a runtime constant

    const int z { x + y };       // x + y is a runtime expression, so z is a runtime const

    return 0;
}
```