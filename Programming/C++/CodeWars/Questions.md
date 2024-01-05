```cpp
// & - address of
// * - de-reference 

// Is a reference to a reference possible? 


// Why can we use std::tolower and std::toupper on a const
bool isPalindrom (const std::string& str)
{
	for (int i{0}; i<str.size(); i++) {
		if(std::tolower(str[i]) != std::tolower(str[str.size()-i-1])) {
			return false; 
		}	
	}
	return true;
}
```