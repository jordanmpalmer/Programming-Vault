## Constructors and Rule of Five

The Rule of Five is a modern extension to the Rule of Three. The Rule of Five states that if a type ever needs one of the following, then it must have all five.

- copy constructor
- copy assignment
- destructor
- move constructor
- move assignment

```cpp
class Deep {
private:
	int *data;                     // Pointer
	char *str;                     // Pointer to a char[] that holds C-style string
public:
	Deep(int d);                   // Constructor
	Deep(const Deep &source);      // Copy Constructor 
	Deep(Deep &&source);           // Move Constructor 
	~Deep();                       // Destructor
	
	Deep& operator=(const Deep &rhs) // Copy assignment
	Deep& operator=(Deep &&rhs)      // Move assignment
}

// Constructor 
Deep::Deep(int d, char str) {
	data = new int;
	*data = d;
}


// Copy constructor
Deep::Deep(const Deep &source) {
	data = new int;       // allocate storage
	*data = *source.data;
}
// **OR** Optionally delegate to constructor
Deep::Deep(const Deep &source) 
	:Deep{*source.data}{
}


// Move constructor
Move::Move (Move &&source) 
	: data{source.data} {
		source.data = nullptr;
}


// Destructor 
Deep::~Deep() {
	delete data;       // free storage
}


// Copy assignment
Deep& Deep::operator=(const Deep &rhs) {
    if (this == &rhs)
        return *this;
    delete [] str;
    str = new char[strlen(rhs.str) + 1];
    strcpy(str, rhs.str);
    return *this;
}

// Move assignment
Deep& Deep::operator=(Deep &&rhs) {
    if(this == &rhs)
        return *this;
    delete [] str;
    data = rhs.str;
    rhs.str = nullptr;
    return *this;
}
```

```cpp 
int main() {
	
    Deep firstDeep { 5, "Hello" };
    Deep secondDeep;
    secondDeep = firstDeep; // calls overloaded copy assignment
	
	return 0;
}
```