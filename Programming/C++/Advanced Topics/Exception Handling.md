Exception handling
- Dealing with extraordinary situations
- Indicates that an extraordinary situation has been detected or has occurred
- Program can deal with the extraordinary situations in a suitable manner

What causes exceptions? 
- Insufficient resources
- Missing resources
- Invalid operations
- Range violations
- Underflows and overflows
- Illegal data and many others

Exception safe
- When your code handles exceptions

Terminology 
- Exception
	- An object or primitive type that signals that an error has occurred
- Throwing an exception (raising an exception)
	- Your code detects that an error has occurred or will occur
	- The place where the error occurred may not know how to handle the error
	- Code can throw an exception describing the error to another part of the program that knows how to handle the error
- Catching an exception (handle the exception)
	- Code that handles the exception
	- May or may not cause the program to terminate

```cpp
throw
```
- throws an exception
- followed by an argument


```cpp
try { code that may throw an exception}
```
- you place code that may throw an exception in a try block
- If the code throws an exception the try block is exited
- The thrown exception is handled by a catch handler
- If no catch handler exists the program terminates

```cpp
catch(Exception ex) { code to handle the exception}
```
- Code that handles the exception
- Can have multiple catch handlers
- May or may not cause the program to terminate

Divide by zero example
- What happens if total is zero?
	- crash, overflow? 
	- it depends

```cpp
double average {};
average = sum / total;
```

```cpp
try 
{
	if (//code that might throw an exception)
		throw 0;
	std::cout << "it worked";
}
catch (int& ex) 
{
	std::cerrr << "Sorry, you can't do that";
}
```

## Throwing an exception from a function

What do we return if total is zero? 
```cpp
// bad
double calculate_avg(int sum, int total) 
{
	return static_cast<double>(sum) / total;
}

// improved
double calculate_avg(int sum, int total) 
{
	if (total == 0)
		throw 0;
	return static_cast<double>(sum) / total;
}

try 
{
	average = calculate_avg(sum, total);
	std::cout << average << std::endl;
}
catch (int& ex)
{
	std::cerr << "You can't divide by zero";
}

std::cout << "Bye";

```

## Multiple Exceptions

```cpp
in function
if (gallons == 0)
	throw 0;
if (miles < 0 || gallons < 0)
	throw std::string{"Negative value error"};

try 
{
	the function
}
catch (int& ex)
{
	std::cerr << "Error";
}
catch (std::string& ex)
{
	std::cerr << ex;
}
catch (...)
{
	std::cerr << "Catch all, unknown exception";
}
```