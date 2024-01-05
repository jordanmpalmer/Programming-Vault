std::string
std::vector
arrays
C-style strings

| Category       | Type           | Minimum Size | Typical Size       |
| -------------- | -------------- | ------------ | ------------------ |
| Boolean        | bool           | 1 byte       | 1 byte             |
| character      | char           | 1 byte       | 1 byte             |
| wchar_t        |                | 1 byte       | 2 or 4 bytes       |
| char8_t        |                | 1 byte       | 1 byte             |
| char16_t       |                | 2 bytes      | 2 bytes            |
| char32_t       |                | 4 bytes      | 4 bytes            |
| integer        | short          | 2 bytes      | 2 bytes            |
| int            |                | 2 bytes      | 4 bytes            |
| long           |                | 4 bytes      | 4 or 8 bytes       |
| long long      |                | 8 bytes      | 8 bytes            |
| floating point | float          | 4 bytes      | 4 bytes            |
| double         |                | 8 bytes      | 8 bytes            |
| long double    |                | 8 bytes      | 8, 12, or 16 bytes |
| pointer        | std::nullptr_t | 4 bytes      | 4 or 8 bytes       |

---

| Type specifier         | Equivalent type                | Width in bits C++ standard |
| ---------------------- | ------------------------------ | -------------------------- |
| signed char            | signed char                    | at least  **8** (2 bytes)         |
| unsigned char          | unsigned char                  |                            |
| short                  | short int                      | at least  **16**           |
| short int              |                                |                            |
| signed short           |                                |                            |
| signed short int       |                                |                            |
| unsigned short         | unsigned short int             |                            |
| unsigned short int     |                                |                            |
| int                    | int                            | at least  **16**           |
| signed                 |                                |                            |
| signed int             |                                |                            |
| unsigned               | unsigned int                   |                            |
| unsigned int           |                                |                            |
| long                   | long int                       | at least  **32**           |
| long int               |                                |                            |
| signed long            |                                |                            |
| signed long int        |                                |                            |
| unsigned long          | unsigned long int              |                            |
| unsigned long int      |                                |                            |
| long long              | long long int  (C++11)         | at least  **64**           |
| long long int          |                                |                            |
| signed long long       |                                |                            |
| signed long long int   |                                |                            |
| unsigned long long     | unsigned long long int (C++11) |                            |
| unsigned long long int |                                |                            |




![[DatatypesInC.png]]

![[TypeTable.png]]

![[TypeTable2.png]]


#### std::nullptr_t (optional)

Since `nullptr` can be differentiated from integer values in function overloads, it must have a different type. So what type is `nullptr`? The answer is that `nullptr` has type `std::nullptr_t` (defined in header <cstddef>). `std::nullptr_t` can only hold one value: `nullptr`! While this may seem kind of silly, it’s useful in one situation. If we want to write a function that accepts only a `nullptr` literal argument, we can make the parameter a `std::nullptr_t`.

```cpp
#include <iostream>
#include <cstddef> // for std::nullptr_t

void print(std::nullptr_t)
{
    std::cout << "in print(std::nullptr_t)\n";
}

void print(int*)
{
    std::cout << "in print(int*)\n";
}

int main()
{
    print(nullptr); // calls print(std::nullptr_t)

    int x { 5 };
    int* ptr { &x };

    print(ptr); // calls print(int*)

    ptr = nullptr;
    print(ptr); // calls print(int*) (since ptr has type int*)

    return 0;
}
```

In the above example, the function call `print(nullptr)` resolves to the function `print(std::nullptr_t)` over `print(int*)` because it doesn’t require a conversion.