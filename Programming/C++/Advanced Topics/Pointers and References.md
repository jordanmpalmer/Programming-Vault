### Declaring a pointer
```cpp
int *int_ptr {};
double* double_ptr {nullptr};
char *int_ptr {nullptr};
string *string_ptr {nullptr};
```

Make sure to initialize so it doesn't have garbage data

### Access a pointer 
```cpp
int_ptr // will show the address stored in the pointer
&int_ptr // will show the address of the pointer
*int_ptr // will show the value at the pointers address

```

### Storing value in pointer
```cpp
double high_temp {100.7};
double *double_ptr {nullptr};

double_ptr = &high_temp;
```

### Dereference pointer
```cpp 
	cout << *temp_ptr // Will output the temperature value
```

### Dynamic Memory Allocation
```cpp
int *array_ptr {nullptr};
array_ptr = new int[size]; // allocate an integer on the heap

delete [] array_ptr; // frees the allocated storage
```

- The value of an array name is the address of the first element in the array

| Subscript Notation   | Offset Notation          |
| -------------------- | ------------------------ |
| array_name\[index]   | \*(array_name + index)   |
| pointer_name\[index] | \*(pointer_name + index) |

- Ptr++ will add one of the size of whatever pointing to (e.g. int = 4)

Qualifying pointers as const
- Pointers to constants
- Constant pointers
- Constant pointers to constants

### Pass by Reference

```cpp

void double_data(int *int_ptr) {
	*int_ptr *= 2;
}

int main() {
	int value {10};
	cout << value << endl; // 10

	double_data(&value);

	cout << value << endl; // 20
}
```

Passing a pointer
```cpp 
void test_function(std::string *Test) {
	*Test = "Changed";
	std::cout << *Test << std::endl;
}

int main() {
	std::string ptrString = "Initial";
		
	test_function(Test);
	
	return 0;
}
```

### Returning a Pointer

```cpp
int *largest_int(int *int_ptr1, int *int_ptr2) {
	if (*int_ptr1 > *int_ptr2)
		return int_ptr1;
	else
		return int_ptr2;
}
```

Never return a pointer to a local variable 

### Returning Dynamically Allocated Memory
```cpp
int *create_array(size_t size, int init_value = 0) {
	int *new_storage {nullptr};

	new_storage = new int[size];
	for (size_t i{0};i<size;++i)
		*(new_storage + i) = init+value;
	return new_storage
}

int main() {
	int *my_array; // will be allocated  by the function

	my_array = create_array(100,20); // create the array

	 // use it

	delete [] my_array // be sure to free the storage
	return 0;
}
```

### l-values and r-values

l-values
- Values that have names and are addressable
- Modifiable if they are not constants
```cpp
int x {100}; // x is an l-value
x = 1000;
x = 1000 + 20;

std::string name; // name is an l-value
name = "Frank";
```

```cpp
100 = x; // 100 is NOT an l-value
(1000+20) = x; // (1000 + 20) is NOT an l-value

std::string name;
name = "Frank";
"Frame" = name; // "Frank" is NOT an l-value
```

r-values
- A value that is not an l-value
- On the right-hand side of an assignment expression
- A literal
- A temporary which is intended to be non-modifiable
- r-values can be assigned to l-values explicitly 

```cpp
int x {100};  // 100 is an r-value
int y = x + 200; // (x+200) is an r-value

std::string name; 
name = "Frank"; // "Frank" is an r-value

int max_num = max(20, 30); // max(20,30) is an r-value
```

l-value references
```cpp
int x {100};

int &ref1 = x; // ref1 is reference to l-value
ref1 = 1000;

int &ref2 = 100 // Error 100 is an r-value
```

```cpp
int square(int &n) {
	return n*n;
}

int num {10};

square(num);   // OK
square(5);     // Error - can't reference r-value 5
```



