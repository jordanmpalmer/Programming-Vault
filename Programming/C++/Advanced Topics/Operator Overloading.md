
![[Operator-Overloading.png]]

### Overloading the Assignment Operator
```cpp

// Copy assignment
Mystring& Mystring::operator=(const Mystring &rhs) {
    std::cout << "Copy assignment" << std::endl;
    if (this == &rhs)
        return *this;
    delete [] str;
    str = new char[strlen(rhs.str) + 1];
    strcpy(str, rhs.str);
    return *this;
}


Mystring a {"Hello"};
a = Mystring{"Hola"}; // move assignment
a = "Bonjour";        // move assignment

// Move assignment
Mystring& Mystring::operator=(Mystring &&rhs) {
    std::cout << "Using move assignment" << std::endl;
    if(this == &rhs)
        return *this;
    delete [] str;
    str = rhs.str;
    rhs.str = nullptr;
    return *this;
}
```

### Overloading Operators as Member Functions

##### Unary Operators (++, --, -, !)
if you need to return you return by value

```cpp
ReturnType Type::operatorOp();

Number Number::operator-() const;
Number Number::operator++();        // pre-increment
Number Number::operator++(int);     // post-increment
Number Number::operator!() const;

Number n1 {100};
Number n2 = -n1;        // n1.operator-()
n2 = ++n1;              // n1.operator++()
n2 = n1++;              // n1.operator++(int)


Mystring Mystring::operator-() const {
	char *buff = new char[std::strlen(str) + 1];
	std::strcpy(buff, str);
	for (size_t i=0; i<std::strlen(buff); i++)
		buff[i] = std::tolower(buff[i]);
	Mystring temp {buff};
	delete [] buff;
	return temp;
}
```

##### Binary Operators (+, -, \==, !=, <, >, etc.)
== 
```cpp
ReturnType Type::operatorOp(const Type &rhs);

Number Number::operator+(const Type &rhs) const;
Number Number::operator-(const Type &rhs) const;     
bool Number::operator==(const Type &rhs) const;    
bool Number::operator<(const Type &rhs) const;

Number n1 {100}, n2 {200};
Number n3 = n1 + n2;        // n1.operator+()
n3 = n1 -n1;                // n1.operator-()
if (n1 == n2)...            // n1.operator==(int)


bool Mystring::operator==(const Mystring &rhs) const {
	if (std::strcmp(str, rhs.str) == 0)
		return true;
	else
		return false;
}
```

Addition
```cpp
// if (s1 == s2)


Mystring larry {"Larry"};
Mystring moe{"Moe}"};
Mystring stooges {" is one of the three stooges"};

Mystring result = larry + stooges;
				// larry.operator+(stooges)
result = moe + " is also a stooge";
				// moe.operator+(" is also a stooge")

result = "Moe" + stooges; // "Moe".operator+(stooges) ERROR


Mystring Mystring::operator+(const Mystring &rhs) const {
	size_t buff_size = std::strlen(str) + 
					   std::strlen(rhs.str) + 1;
	char *buff = new char[buff_size];
	std::strcpy(buff, str);
	std::strcat(buff, rhs.str);
	Mystring temp {buff};
	delete [] buff;
	return temp;
}
```

### Overloading Operators as Global Functions

```cpp
// Unary operators
ReturnType operatorOp(Type &obj);

Number Number::operator-(const Number &obj);
Number Number::operator++(Number &obj);       // pre-increment
Number Number::operator++(Number &obj, int);  // post-increment
Number Number::operator!(const Number &obj);

Number n1 {100};
Number n2 = -n1;        // n1.operator-(n1)
n2 = ++n1;              // n1.operator++(n1)
n2 = n1++;              // n1.operator++(n1,int)

Mystring operator-(const Mystring &obj) {
	char *buff = new char[std::strlenn(obj.str) + 1];
	std::strcpy(buff, obj.str);
	for (size_t i{0}; i<std::strlen(buff); i++)
		buff[i] = std::tolower(buff[i]);
	Mystring temp {buff};
	delete [] buff;
	return temp;
}


// Binary operators
ReturnType operatorOp(const Type &lhs, const Type &rhs);

Number operator+(const Type &lhs, const Type &rhs) const;
Number operator-(const Type &lhs, const Type &rhs) const;     
bool operator==(const Type &lhs, const Type &rhs) const;    
bool operator<(const Type &lhs, const Type &rhs) const;

Number n1 {100}, n2 {200};
Number n3 = n1 + n2;        // n1.operator+(n1,n2)
n3 = n1 -n1;                // n1.operator-(n1,n2)
if (n1 == n2)...            // n1.operator==(n1,n2)

bool operator==(const Mystring &lhs, const Mystring &rhs) {
	if (std::strcmp(lhs.str, rhs.str) == 0)
		return true;
	else 
		return false;
}


// concatenation
Mystring larry {"Larry"};
Mystring moe{"Moe"};
Mystring stooges {" is one of the three stooges"};

Mystring result = larry + stooges;
				// operator+(larry, stooges);

result = moe + " is also a stooge";
			    // operator+(moe, " is also a stooge");

result = "Moe" + stooges;  // OK with non-member functions

Mystring operator+(const Mystring& lhs, const Mystring& lhs) {
	size_t buff_size = std::strlen(lhs.str) + 
					   std::strlen(rhs.str) + 1;
	char *buff = new char[buff_size];
	std::strcpy(buff, lhs.str);
	std::strcat(buff, rhs.str);
	Mystring temp {buff};
	delete [] buff;
	return temp;
}
```

### Stream insertion and extraction operators (<<, >>)

```cpp
std::ostream &operator<<(std::ostream &os, const Mystring &obj) {
	os << obj.str;    // if friend function
	// os << obj.get_str(); // if not friend function
	return os;
}

std::istream &operator>>(std::istream &is, Mystring &obj) {
	char *buff = new char[1000];
	is >> buff;
	obj = Mystring{buff}; // if you have a copy or move assignment
	delete [] buff;
	return is;
}
```