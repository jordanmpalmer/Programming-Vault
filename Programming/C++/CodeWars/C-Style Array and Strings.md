```cpp
#include <iostream>
#include <iterator> // for std::size and std::ssize

int main()
{
	int arr[5];    // Members default initialized int elements are left uninitialized)
    int arr[5] {}; // Members value initialized (int elements are zero uninitialized) (preferred)
    int arr[4] { 1, 2 };                // arr[2] and arr[3] are value initialized
    int arr[] { 2, 3, 5, 7, 11 };   // the compiler deduces prime to have length 5
	const int arr[5] { 2, 3, 5, 7, 11 }; // an array of const int

    std::size(arr)                  // returns integral value 5
    std::length(arr)
    std::strcat(,)
    std::strcpy(,)

    return 0;
}
```

```cpp
char str1[8]{};                    // an array of 8 char, indices 0 through 7

const char str2[]{ "string" };     // an array of 7 char, indices 0 through 6
constexpr char str3[] { "hello" }; // an array of 6 const char, indices 0 through 5

printf(const* char)

strlen(string)
```

> [!BEST PRACTICE]
> Prefer omitting the length of a C-style array when explicitly initializing every array element with a value.

```cpp
std::map<char, char> ??
```