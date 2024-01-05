## For function parameters, prefer `std::string_view` over `const std::string&` in most cases [](https://www.learncpp.com/cpp-tutorial/pass-by-const-lvalue-reference/#stringparameter)

One question that comes up often in modern C++: when writing a function that has a string parameter, should the type of the parameter be `const std::string&` or `std::string_view`?

In most cases, `std::string_view` is the better choice, as it can handle a wider range of argument types efficiently.

```cpp
void doSomething(const std::string&);
void doSomething(std::string_view);   // prefer this in most cases
```

There are a few cases where using a `const std::string&` parameter may be more appropriate:

- If you’re using C++14 or older, `std::string_view` isn’t available.
- If your function needs to call some other function that takes a C-style string or `std::string` parameter, then `const std::string&` may be a better choice, as `std::string_view` is not guaranteed to be null-terminated (something that C-style string functions expect) and does not efficiently convert back to a `std::string`.

> [!NOTE]
> Best practice
> 
> Prefer passing strings using `std::string_view` (by value) instead of `const std::string&`, unless your function calls other functions that require C-style strings or `std::string` parameters.

## Why `std::string_view` parameters are more efficient than `const std::string&` Advanced

In C++, a string argument will typically be a `std::string`, a `std::string_view`, or a C-style string/string literal.

As reminders:

- If the type of an argument does not match the type of the corresponding parameter, the compiler will try to implicitly convert the argument to match the type of the parameter.
- Converting a value creates a temporary object of the converted type.
- Creating (or copying) a `std::string_view` is inexpensive, as `std::string_view` does not make a copy of the string it is viewing.
- Creating (or copying) a `std::string` can be expensive, as each `std::string` object makes a copy of the string.

Here’s a table showing what happens when we try to pass each type:

|Argument Type|std::string_view parameter|const std::string& parameter|
|---|---|---|
|std::string|Inexpensive conversion|Inexpensive reference binding|
|std::string_view|Inexpensive copy|Requires expensive explicit conversion to `std::string`|
|C-style string / literal|Inexpensive conversion|Expensive conversion|

With a `std::string_view` value parameter:

- If we pass in a `std::string` argument, the compiler will convert the `std::string` to a `std::string_view`, which is inexpensive, so this is fine.
- If we pass in a `std::string_view` argument, the compiler will copy the argument into the parameter, which is inexpensive, so this is fine.
- If we pass in a C-style string or string literal, the compiler will convert these to a `std::string_view`, which is inexpensive, so this is fine.

As you can see, `std::string_view` handles all three cases inexpensively.

With a `const std::string&` reference parameter:

- If we pass in a `std::string` argument, the parameter will reference bind to the argument, which is inexpensive, so this is fine.
- If we pass in a `std::string_view` argument, the compiler will refuse to do an implicit conversion, and produce a compilation error. We can use `static_cast` to do an explicit conversion (to `std::string`), but this conversion is expensive (since `std::string` will make a copy of the string being viewed). Once the conversion is done, the parameter will reference bind to the result, which is inexpensive. But we’ve made an expensive copy to do the conversion, so this isn’t great.
- If we pass in a C-style string or string literal, the compiler will implicitly convert this to a `std::string`, which is expensive. So this isn’t great either.

Thus, a `const std::string&` parameter only handles `std::string` arguments inexpensively.

The same, in code form:

```cpp
#include <iostream>
#include <string>
#include <string_view>

void printSV(std::string_view sv)
{
    std::cout << sv << '\n';
}

void printS(const std::string& s)
{
    std::cout << s << '\n';
}

int main()
{
    std::string s{ "Hello, world" };
    std::string_view sv { s };

    // Pass to `std::string_view` parameter
    printSV(s);              // ok: inexpensive conversion from std::string to std::string_view
    printSV(sv);             // ok: inexpensive copy of std::string_view
    printSV("Hello, world"); // ok: inexpensive conversion of C-style string literal to std::string_view

    // pass to `const std::string&` parameter
    printS(s);              // ok: inexpensive bind to std::string argument
    printS(sv);             // compile error: cannot implicit convert std::string_view to std::string
    printS(static_cast<std::string>(sv)); // bad: expensive creation of std::string temporary
    printS("Hello, world"); // bad: expensive creation of std::string temporary

    return 0;
}
```

Additionally, we need to consider the cost of accessing the parameter inside the function. Because a `std::string_view` parameter is a normal object, the string being viewed can be accessed directly. Accessing a `std::string&` parameter requires an additional step to get to the referenced object before the string can be accessed.