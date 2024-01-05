
| Syntax            | Explanation                                                       |
| ----------------- | ----------------------------------------------------------------- |
| `&num = val`      | num now references val, what num is being pointed at can't change |
| `int *var_ptr`    | declare a pointer                                                 |
| `var_ptr = &var2` | assign a value in the pointers memory                             |
|                   |                                                                   |
|                   |                                                                   |
|                   |                                                                   |

Make the function variable a reference to avoid recreating variable
```cpp
std::string reverse_string(const std::string &str)

reverse_string(string);
```

