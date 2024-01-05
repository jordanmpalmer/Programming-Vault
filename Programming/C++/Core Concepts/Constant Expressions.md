## Constant Expressions 
A **constant expression** is an expression that can be evaluated by the compiler at compile-time. To be a constant expression, all the values in the expression must be known at compile-time (and all the operators and functions called must support compile-time evaluation).

When the compiler encounters a constant expression, it _may_ evaluate the expression at compile-time, and then replace the constant expression with the result of the evaluation.

In the above program, the expression `3 + 4` is a constant expression. So when this program is compiled, the compiler may replace constant expression `3 + 4` with the resulting value `7`. In other words, under the as-if rule, the compiler may actually compile this:

```cpp
#include <iostream>

int main()
{
	int x { 7 };
	std::cout << x << '\n';

	return 0;
}
```

This program produces the same output (`7`) as the prior version, but the resulting executable no longer needs to spend CPU cycles calculating `3 + 4` at runtime! Even better, we don’t need to do anything to enable this behavior (besides have optimizations turned on).

Note that the expression `std::cout << x` is not a constant expression, because our program can’t output values to the console at compile-time. So this expression will always evaluate at runtime. An expression that must be evaluated at runtime is sometimes called a **runtime expression**.

> [!TIP]
> Evaluating constant expressions at compile-time makes our compilation take longer (because the compiler has to do more work), but such expressions only need to be evaluated once (rather than every time the program is run). The resulting executables are faster and use less memory.
> 
> The ability for C++ to perform compile-time evaluation is one of the most important and evolving areas of modern C++.

## Another optimization opportunity

There is another inefficiency in the program above: The program allocates memory for `x`, stores the value `7` in that memory, and then in the subsequent statement goes back to memory to get the value of `x` (7) to be printed. Since the value of `x` never changes, this memory access is wasteful.

In other words, under the as-if rule, the compiler could optimize the above program into the following:

```cpp
#include <iostream>

int main()
{
	std::cout << 7 << '\n';

	return 0;
}
```

But in order to make this optimization, the compiler has to be sure that `x` isn’t changed between when it is defined and when it is used. Because `x`is non-constant, the compiler will have to do its own analysis to determine whether this is the case. While a sophisticated modern optimizing compiler would be able to do this (at least in this simple case), less sophisticated compilers or more complicated cases will impede this optimization.

Unlike the constant expression optimization (which we essentially got for free), this optimization we may or may not get for free.

However, for a trivial amount of work, we can help the compiler so that it is much more likely to perform this optimization.

## The `constexpr` keyword

When you declare a const variable using the `const` keyword, the compiler will implicitly keep track of whether it’s a runtime or compile-time constant. In most cases, this doesn’t matter for anything other than optimization purposes, but there are a few cases where C++ requires a constant expression (we’ll cover these cases later as we introduce those topics). And only compile-time constant variables can be used in a constant expression.

Because compile-time constants also allow for better optimization (and have little downside), we typically want to use compile-time constants wherever possible.

When using `const`, our variables could end up as either a compile-time const or a runtime const, depending on whether the initializer is a constant expression or not. In some cases, it can be hard to tell whether a const variable is a compile-time const (and usable in a constant expression) or a runtime const (and not usable in a constant expression).

For example:

```cpp
int x { 5 };       // not const at all
const int y { x }; // obviously a runtime const (since initializer is non-const)
const int z { 5 }; // obviously a compile-time const (since initializer is a constant expression)
const int w { getValue() }; // not obvious whether this is a runtime or compile-time const
```

In the above example, `w` could be either a runtime or a compile-time const depending on how `getValue()` is defined. It’s not at all clear!

Fortunately, we can enlist the compiler’s help to ensure we get a compile-time const where we desire one. To do so, we use the `constexpr`keyword instead of `const` in a variable’s declaration. A **constexpr** (which is short for “constant expression”) variable can only be a compile-time constant. If the initialization value of a constexpr variable is not a constant expression, the compiler will error.

For example:

```cpp
#include <iostream>

int five()
{
    return 5;
}

int main()
{
    constexpr double gravity { 9.8 }; // ok: 9.8 is a constant expression
    constexpr int sum { 4 + 5 };      // ok: 4 + 5 is a constant expression
    constexpr int something { sum };  // ok: sum is a constant expression

    std::cout << "Enter your age: ";
    int age{};
    std::cin >> age;

    constexpr int myAge { age };      // compile error: age is not a constant expression
    constexpr int f { five() };       // compile error: return value of five() is not a constant expression

    return 0;
}
```

> [!Best Practice]
> Any variable that should not be modifiable after initialization and whose initializer is known at compile-time should be declared as `constexpr`.  
> Any variable that should not be modifiable after initialization and whose initializer is not known at compile-time should be declared as `const`.
> 
> Caveat: In the future we will discuss some types that are not currently compatible with `constexpr` (including `std::string`, `std::vector`, and other types that use dynamic memory allocation). For constant objects of these types, use `const` instead.

> [!Author's Note]
> Many of the examples on this site were written prior to the best practice to use constexpr -- as a result, you will note that many examples do not follow the above best practice. We are currently in the process of updating non-compliant examples as we run across them.

## Const and constexpr function parameters

Normal function calls are evaluated at runtime, with the supplied arguments being used to initialize the function’s parameters. This means `const`function parameters are treated as runtime constants, even when the supplied argument is a compile-time constant.

Because constexpr objects must be initialized with a compile-time constant (not a runtime constant), function parameters cannot be declared as `constexpr`.

> [!Related Content]
> C++ does support functions that can be evaluated at compile-time (and thus can be used in constant expressions). We discuss these in lesson [5.8 -- Constexpr and consteval functions](https://www.learncpp.com/cpp-tutorial/constexpr-and-consteval-functions/).
> 
> C++ also supports a way to pass compile-time constants to a function. We discuss these in lesson [10.18 -- Non-type template parameters](https://www.learncpp.com/cpp-tutorial/non-type-template-parameters/).

## When are constant expressions actually evaluated?

The compiler is only _required_ to evaluate constant expressions at compile-time in contexts that require a constant expression (such as the initializer of a compile-time constant):


```cpp
constexpr int x { 3 + 4 }; // 3 + 4 will always evaluate at compile time
const int x { 3 + 4 };     // 3 + 4 will always evaluate at compile time
```

In contexts that do not require a constant expression, the compiler may choose whether to evaluate a constant expression at compile-time or at runtime.

```cpp
int x { 3 + 4 }; // 3 + 4 may evaluate at compile-time or runtime
```

In the above variable definition, `x` is not a constexpr variable and the initialization value does not need to be known at compile-time. Thus the compiler is free to choose whether to evaluate `3 + 4` at compile-time or runtime.

Even though it is not strictly required, modern compilers will _usually_ evaluate a constant expression at compile-time because it is an easy optimization and more performant to do so.

## Constant folding for constant subexpressions

Consider the following example:

```cpp
#include <iostream>

int main()
{
	constexpr int x { 3 + 4 }; // 3 + 4 is a constant expression
	std::cout << x << '\n';    // this is a runtime expression

	return 0;
}
```

`3 + 4` is a constant expression, so the compiler will evaluate `3 + 4` at compile-time, and replace it with value `7`. Because `x` is a compile-time constant, the compiler will likely optimize `x` out of the above program altogether, replacing `std::cout << x << '\n'` with `std::cout << 7 << '\n'`. The output expression will execute at runtime.

However, because `x` is only used once, it’s more likely we’d write the program like this in the first place:

```cpp
#include <iostream>

int main()
{
	std::cout << 3 + 4 << '\n'; // this is a runtime expression

	return 0;
}
```

Since the full expression `std::cout << 3 + 4 << '\n'` is not a constant expression, it’s reasonable to wonder whether the constant subexpression `3 + 4` will still be optimized at compile-time.

> [!Related Content]
> We define the term “subexpression” in lesson [1.10 -- Introduction to expressions](https://www.learncpp.com/cpp-tutorial/introduction-to-expressions/).

The answer is generally “yes”. Compilers have long been able to optimize constant subexpressions, even when the full expression is a runtime expression. This optimization process is called “constant folding”.

Making our variables constexpr ensures that those variables are eligible for constant folding when they are used in constant subexpressions.

## [# Constexpr and consteval functions](https://www.learncpp.com/cpp-tutorial/constexpr-and-consteval-functions/)