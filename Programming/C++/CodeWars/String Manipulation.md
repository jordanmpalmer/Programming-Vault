https://en.cppreference.com/w/cpp/string/basic_string
```cpp
// Capacity
string.empty()        // checks if a string is empty
string.length()       // length of string
string.size()         // essentially length of string
string.max_size()     // returns the strings max size
string.reserve()      // reserves storage 

// Element access
string.at()           // accesses the specified character with bounds checking
string[]              // accesses the specified character
string.front()        // access first character
string.back()         // access last character
string.data()         // returns pointer to first element

// Iterators 
string.begin()        // returns iterator to start
string.end()          // returns iterator to end

// Modifiers 
string.clear()        // clears string
string.insert()       // inserts characters
string.erase()        // removes characters
string.push_back()    // appends character to end
string.pop_back()     // removes last character
string.append()       // appends chars to end
string.replace()      // replace specified portion of string
string.copy()         // copies characters 
string.resize()       // changes number of characters stored
string.swap()         // swaps the content

// Search
string.find()         // finds first instance of character

// Operations
string.compare()      // compares two strings
string.starts_with()  // checks prefix C++20
string.ends_with()    // checks suffix C++20
string.contains()     // checks for a given char or substring C++23
string.substring()    // returns substring

+ operator concatenates strings or string and char
+ = add to end of string

std::replace(,,,)     // replace part of string
std::accumulate(,,)   // not sure
std::tolower(str[index])        // lower case
std::toupper()        // upper case
std::strcmp()         // compare two strings

string_name(vec.begin(),vec.end())

strcpy(new_str,src_str)

std::string::size_type


for (char& c : str)
```