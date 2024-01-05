### An introduction to explicit type conversion via the static_cast operator

Back to our most recent `print()` example, what if we _intentionally_ wanted to pass a double value to a function taking an integer (knowing that the converted value would drop any fractional component?) Turning off “treat warnings as errors” just to make our program compile is a bad idea, because then we’ll have warnings every time we compile (which we will quickly learn to ignore), and we risk overlooking warnings about more serious issues.

C++ supports a second method of type conversion, called explicit type conversion. **Explicit type conversion** allow us (the programmer) to explicitly tell the compiler to convert a value from one type to another type, and that we take full responsibility for the result of that conversion (meaning that if the conversion results in the loss of value, it’s our fault).

To perform an explicit type conversion, in most cases we’ll use the `static_cast` operator. The syntax for the `static cast` looks a little funny:

static_cast<new_type>(expression)

static_cast takes the value from an expression as input, and returns that value converted into the type specified by _new_type_ (e.g. int, bool, char, double).

> [!NOTE]
> **Key insight**
> Whenever you see C++ syntax (excluding the preprocessor) that makes use of angled brackets (<>), the thing between the angled brackets will most likely be a type. This is typically how C++ deals with code that need a parameterized type.

Let’s update our prior program using `static_cast`:

```cpp
#include <iostream>

void print(int x)
{
	std::cout << x << '\n';
}

int main()
{
	print( static_cast<int>(5.5) ); // explicitly convert double value 5.5 to an int

	return 0;
}
```

Because we’re now explicitly requesting that double value `5.5` be converted to an `int` value, the compiler will not generate a warning about a possible loss of data upon compilation (meaning we can leave “treat warnings as errors” enabled).

> [!NOTE]
> **Related content**
> C++ supports other types of casts. We talk more about the different types of casts in future lesson [10.6 -- Explicit type conversion (casting) and static_cast](https://www.learncpp.com/cpp-tutorial/explicit-type-conversion-casting-and-static-cast/).

Using static_cast to convert char to int

In the lesson on chars [4.11 -- Chars](https://www.learncpp.com/cpp-tutorial/chars/), we saw that printing a char value using `std::cout` results in the value being printed as a char:

```cpp
#include <iostream>

int main()
{
    char ch{ 97 }; // 97 is ASCII code for 'a'
    std::cout << ch << '\n';

    return 0;
}
```

This prints:

> a

If we want to print the integral value instead of the char, we can do this by using `static_cast` to cast the value from a `char` to an `int`:

```cpp
#include <iostream>

int main()
{
    char ch{ 97 }; // 97 is ASCII code for 'a'
    std::cout << ch << " has value " << static_cast<int>(ch) << '\n'; // print value of variable ch as an int

    return 0;
}
```

This prints:

>a has value 97

It’s worth noting that the argument to _static_cast_ evaluates as an expression. When we pass in a variable, that variable is evaluated to produce its value, and that value is then converted to the new type. The variable itself is _not_ affected by casting its value to a new type. In the above case, variable `ch` is still a char, and still holds the same value even after we’ve cast its value to an `int`.

### Converting unsigned numbers to signed numbers

To convert an unsigned number to a signed number, you can also use the `static_cast` operator:

```cpp
#include <iostream>

int main()
{
    unsigned int u { 5 };
    int s { static_cast<int>(u) }; // return value of variable u as an int

    std::cout << s << '\n';
    return 0;
}
```

The `static_cast` operator doesn’t do any range checking, so if you cast a value to a type whose range doesn’t contain that value, undefined behavior will result. Therefore, the above cast from `unsigned int` to `int` will yield unpredictable results if the value of the `unsigned int` is greater than the maximum value a signed `int` can hold.

> [!NOTE]
> Warning
> The static_cast operator will produce undefined behavior if the value being converted doesn’t fit in range of the new type.

### std::int8_t and std::uint8_t likely behave like chars instead of integers

As noted in lesson [4.6 -- Fixed-width integers and size_t](https://www.learncpp.com/cpp-tutorial/fixed-width-integers-and-size-t/), most compilers define and treat `std::int8_t` and `std::uint8_t` (and the corresponding fast and least fixed-width types) identically to types `signed char` and `unsigned char` respectively. Now that we’ve covered what chars are, we can demonstrate where this can be problematic:

```cpp
#include <cstdint>
#include <iostream>

int main()
{
    std::int8_t myInt{65};      // initialize myInt with value 65
    std::cout << myInt << '\n'; // you're probably expecting this to print 65

    return 0;
}
```

Because `std::int8_t` describes itself as an int, you might be tricked into believing that the above program will print the integral value `65`. However, on most systems, this program will print `A` instead (treating `myInt` as a `signed char`). However, this is not guaranteed (on some systems, it may actually print `65`).

If you want to ensure that a `std::int8_t` or `std::uint8_t` object is treated as an integer, you can convert the value to an integer using `static_cast`:

```cpp
#include <cstdint>
#include <iostream>

int main()
{
    std::int8_t myInt{65};
    std::cout << static_cast<int>(myInt) << '\n'; // will always print 65

    return 0;
}
```

In cases where `std::int8_t` is treated as a char, input from the console can also cause problems:

```cpp
#include <cstdint>
#include <iostream>

int main()
{
    std::cout << "Enter a number between 0 and 127: ";
    std::int8_t myInt{};
    std::cin >> myInt;

    std::cout << "You entered: " << static_cast<int>(myInt) << '\n';

    return 0;
}
```

A sample run of this program:

>Enter a number between 0 and 127: 35
>You entered: 51

Here’s what’s happening. When `std::int8_t` is treated as a char, the input routines interpret our input as a sequence of characters, not as an integer. So when we enter `35`, we’re actually entering two chars, `'3'` and `'5'`. Because a char object can only hold one character, the `'3'` is extracted (the `'5'` is left in the input stream for possible extraction later). Because the char `'3'` has ASCII code point 51, the value `51` is stored in `myInt`, which we then print later as an int.

In contrast, the other fixed-width types will always print and input as integral values.