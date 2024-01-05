# 2.7 — Forward declarations and definitions

Take a look at this seemingly innocent sample program:

```cpp
#include <iostream>

int main()
{
    std::cout << "The sum of 3 and 4 is: " << add(3, 4) << '\n';
    return 0;
}

int add(int x, int y)
{
    return x + y;
}
```

You would expect this program to produce the result:

The sum of 3 and 4 is: 7

But in fact, it doesn’t compile at all! Visual Studio produces the following compile error:

add.cpp(5) : error C3861: 'add': identifier not found

The reason this program doesn’t compile is because the compiler compiles the contents of code files sequentially. When the compiler reaches the function call to _add_ on line 5 of _main_, it doesn’t know what _add_ is, because we haven’t defined _add_ until line 9! That produces the error, _identifier not found_.

Older versions of Visual Studio would produce an additional error:


add.cpp(9) : error C2365: 'add'; : redefinition; previous definition was 'formerly unknown identifier'

This is somewhat misleading, given that _add_ wasn’t ever defined in the first place. Despite this, it’s useful to generally note that it is fairly common for a single error to produce many redundant or related errors or warnings. It can sometimes be hard to tell whether any error or warning beyond the first is a consequence of the first issue, or whether it is an independent issues that needs to be resolved separately.

> [!NOTE]
> Best practice
> When addressing compilation errors or warnings in your programs, resolve the first issue listed and then compile again.

To fix this problem, we need to address the fact that the compiler doesn’t know what add is. There are two common ways to address the issue.

### Option 1: Reorder the function definitions

One way to address the issue is to reorder the function definitions so _add_ is defined before _main_:

```cpp
#include <iostream>

int add(int x, int y)
{
    return x + y;
}

int main()
{
    std::cout << "The sum of 3 and 4 is: " << add(3, 4) << '\n';
    return 0;
}
```

That way, by the time _main_ calls _add_, the compiler will already know what _add_ is. Because this is such a simple program, this change is relatively easy to do. However, in a larger program, it can be tedious trying to figure out which functions call which other functions (and in what order) so they can be declared sequentially.

Furthermore, this option is not always possible. Let’s say we’re writing a program that has two functions _A_ and _B_. If function _A_ calls function _B_, and function _B_ calls function _A_, then there’s no way to order the functions in a way that will make the compiler happy. If you define _A_ first, the compiler will complain it doesn’t know what _B_ is. If you define _B_ first, the compiler will complain that it doesn’t know what _A_ is.

### Option 2: Use a forward declaration

We can also fix this by using a forward declaration.

A **forward declaration** allows us to tell the compiler about the existence of an identifier _before_ actually defining the identifier.

In the case of functions, this allows us to tell the compiler about the existence of a function before we define the function’s body. This way, when the compiler encounters a call to the function, it’ll understand that we’re making a function call, and can check to ensure we’re calling the function correctly, even if it doesn’t yet know how or where the function is defined.

To write a forward declaration for a function, we use a **function declaration** statement (also called a **function prototype**). The function declaration consists of the function’s return type, name, and parameter types, terminated with a semicolon. The names of the parameters can be optionally included. The function body is not included in the declaration.

Here’s a function declaration for the _add_ function:

```cpp
int add(int x, int y); // function declaration includes return type, name, parameters, and semicolon.  No function body!
```

Now, here’s our original program that didn’t compile, using a function declaration as a forward declaration for function _add_:

```cpp
#include <iostream>

int add(int x, int y); // forward declaration of add() (using a function declaration)

int main()
{
    std::cout << "The sum of 3 and 4 is: " << add(3, 4) << '\n'; // this works because we forward declared add() above
    return 0;
}

int add(int x, int y) // even though the body of add() isn't defined until here
{
    return x + y;
}
```

Now when the compiler reaches the call to _add_ in main, it will know what _add_ looks like (a function that takes two integer parameters and returns an integer), and it won’t complain.

It is worth noting that function declarations do not need to specify the names of the parameters (as they are not considered to be part of the function declaration). In the above code, you can also forward declare your function like this:

```cpp
int add(int, int); // valid function declaration
```

However, we prefer to name our parameters (using the same names as the actual function), because it allows you to understand what the function parameters are just by looking at the declaration. Otherwise, you’ll have to locate the function definition.

> [!NOTE]
> Best practice
> Keep the parameter names in your function declarations.


> [!TIP]
> Tip
> You can easily create function declarations by copy/pasting your function’s header and adding a semicolon.

### Why forward declarations?

You may be wondering why we would use a forward declaration if we could just reorder the functions to make our programs work.

Most often, forward declarations are used to tell the compiler about the existence of some function that has been defined in a different code file. Reordering isn’t possible in this scenario because the caller and the callee are in completely different files! We’ll discuss this in more detail in the next lesson ([2.8 -- Programs with multiple code files](https://www.learncpp.com/cpp-tutorial/programs-with-multiple-code-files/)).

Forward declarations can also be used to define our functions in an order-agnostic manner. This allows us to define functions in whatever order maximizes organization (e.g. by clustering related functions together) or reader understanding.

Less often, there are times when we have two functions that call each other. Reordering isn’t possible in this case either, as there is no way to reorder the functions such that each is before the other. Forward declarations give us a way to resolve such circular dependencies.

![Ezoic](https://go.ezodn.com/utilcave_com/ezoicbwa.png "ezoic")

### Forgetting the function body

New programmers often wonder what happens if they forward declare a function but do not define it.

The answer is: it depends. If a forward declaration is made, but the function is never called, the program will compile and run fine. However, if a forward declaration is made and the function is called, but the program never defines the function, the program will compile okay, but the linker will complain that it can’t resolve the function call.

Consider the following program:

```cpp
#include <iostream>

int add(int x, int y); // forward declaration of add()

int main()
{
    std::cout << "The sum of 3 and 4 is: " << add(3, 4) << '\n';
    return 0;
}

// note: No definition for function add
```

In this program, we forward declare _add_, and we call _add_, but we never define _add_ anywhere. When we try and compile this program, Visual Studio produces the following message:

Compiling...
add.cpp
Linking...
add.obj : error LNK2001: unresolved external symbol "int __cdecl add(int,int)" (?add@@YAHHH@Z)
add.exe : fatal error LNK1120: 1 unresolved externals

As you can see, the program compiled okay, but it failed at the link stage because _int add(int, int)_ was never defined.

### Other types of forward declarations

Forward declarations are most often used with functions. However, forward declarations can also be used with other identifiers in C++, such as variables and types. Variables and types have a different syntax for forward declaration, so we’ll cover these in future lessons.

### Declarations vs. definitions

In C++, you’ll frequently hear the words “declaration” and “definition” used, and often interchangeably. What do they mean? You now have enough fundamental knowledge to understand the difference between the two.

A **declaration** tells the _compiler_ about the _existence_ of an identifier and its associated type information. Here are some examples of declarations:

```cpp
int add(int x, int y); // tells the compiler about a function named "add" that takes two int parameters and returns an int.  No body!
int x;                 // tells the compiler about an integer variable named x
```

A **definition** is a declaration that actually implements (for functions and types) or instantiates (for variables) the identifier.

Here are some examples of definitions:

```cpp
int add(int x, int y) // implements function add()
{
    int z{ x + y };   // instantiates variable z

    return z;
}

int x;                // instantiates variable x
```

In C++, all definitions are declarations. Therefore `int x;` is both a definition and a declaration.

Conversely, not all declarations are definitions. Declarations that aren’t definitions are called **pure declarations**. Types of pure declarations include forward declarations for function, variables, and types.

> [!NOTE]
> Author’s note
> In common language, the term “declaration” is typically used to mean “a pure declaration”, and “definition” is used to mean “a definition that also serves as a declaration”. Thus, we’d typically call _int x;_ a definition, even though it is both a definition and a declaration.

When the compiler encounters an identifier, it will check to ensure use of that identifier is valid (e.g. that the identifier is in scope, that it is used in a syntactically valid manner, etc…).

In most cases, a declaration is sufficient to allow the compiler to ensure an identifier is being used properly. For example, when the compiler encounters function call `add(5, 6)`, if it has already seen the declaration for `add(int, int)`, then it can validate that `add` is actually a function that takes two `int` parameters. It does not need to have actually seen the definition for function `add` (which may exist in some other file).

However, there are a few cases where the compiler must be able to see a full definition in order to use an identifier (such as for template definitions and type definitions, both of which we will discuss in future lessons).

Here’s a summary table:

| Term             | Meaning                                                                                 | Examples                                                                 |
| ---------------- | --------------------------------------------------------------------------------------- | ------------------------------------------------------------------------ |
| Definition       | Implements a function or instantiates a variable.<br>Definitions are also declarations. | void foo() { } // function definition  <br>int x; // variable definition |
| Declaration      | Tells compiler about an identifier and its associated type information.                 | void foo(); // function declaration  <br>int x; // variable declaration  |
| Pure declaration | A declaration that isn’t a definition.                                                  | void foo();                                                              |
| Initializer      | Provides an initial value for a defined object.                                         | int x { 2 }; // 2 is the initializer                                     |

### The one definition rule (ODR) [](https://www.learncpp.com/cpp-tutorial/forward-declarations/#ODR)

The **one definition rule** (or ODR for short) is a well-known rule in C++. The ODR has three parts:

- Within a _file_, each function, variable, type, or template can only have one definition. Definitions occurring in different scopes (e.g. local variables defined inside different functions, or functions defined inside different namespaces) do not violate this rule.
- Within a _program_, each function or variable can only have one definition. This rule exists because programs can have more than one file (we’ll cover this in the next lesson). Functions and variables not visible to the linker are excluded from this rule (discussed further in lesson [7.6 -- Internal linkage](https://www.learncpp.com/cpp-tutorial/internal-linkage/)).
- Types, templates, inline functions, and inline variables are allowed to have duplicate definitions in different files, so long as each definition is identical. We haven’t covered what most of these things are yet, so don’t worry about this for now -- we’ll bring it back up when it’s relevant.

Violating part 1 of the ODR will cause the compiler to issue a redefinition error. Violating ODR part 2 will cause the linker to issue a redefinition error. Violating ODR part 3 will cause undefined behavior.

Here’s an example of a violation of part 1:

```cpp
int add(int x, int y)
{
     return x + y;
}

int add(int x, int y) // violation of ODR, we've already defined function add(int, int)
{
     return x + y;
}

int main()
{
    int x{};
    int x{ 5 }; // violation of ODR, we've already defined x
}
```

In this example, function `add(int, int)` is defined twice (in the global scope), and local variable `int x` is defined twice (in the scope of `main()`). The Visual Studio compiler thus issues the following compile errors:

project3.cpp(9): error C2084: function 'int add(int,int)' already has a body
project3.cpp(3): note: see previous definition of 'add'
project3.cpp(16): error C2086: 'int x': redefinition
project3.cpp(15): note: see declaration of 'x'

However, it is not a violation of ODR part 1 for `main()` to have a local variable defined as `int x` and `add()` to also have a function parameter defined as `int x`. These definitions occur in different scopes (in the scope of each respective function), so they are considered to be separate definitions for two distinct objects, not a definition and redefinition of the same object.

> [!NOTE]
> For advanced readers
> Functions that share an identifier but have different sets of parameters are also considered to be distinct functions, so such definitions do not violate the ODR. We discuss this further in lesson [10.10 -- Introduction to function overloading](https://www.learncpp.com/cpp-tutorial/introduction-to-function-overloading/).