Heap vs Stack

Stack is like a stack of books you can add and remove from the top but not from the bottom or middle. Call stack is your functions being executed. If a function is called within a function it must be terminated before terminating the first one. 

Heap is memory you can access from anywhere. Heap is generally used when you have data that will outlive what's on the call stack 

Reference types are always stored in the heap
Value types can be on heap or stack. Local variable gets saved in the call stack, if you call a reference in the call stack the pointer is on the stack but the memory is in the heap.

Exceptions
Static variables will always be on the heap
Async methods, multi threads

```cpp

struct Vector3 {
	float x, y, z;	
	Vector3()
		: x{10},y{11},z{12} {
	}
}; 

int main() {
	// allocate on the stack (physicially close together in memory, on a stack)
	// stack pointer just moves when more space is allocated, then returns address
	// very fast
	int svalue {5};
	int sarray[5] {1,2,3,4,5};
	Vector3 svector;

	// allocate on the heap, random spot in memory
	int* hvalue = new int;
	*hvalue = 5
	int* harray = new int[5];
	harray[0] = 1
	harray[1] = 1
	harray[2] = 1
	harray[3] = 1
	harray[4] = 1
	Vector3* hvector = new Vector3();  // parenthesis are optional

	delete hvalue;
	delete[] harray;
	delete hvector;

	return 0
}
```

| Value Types | Reference Types |
| ----------- | --------------- |
| Struct      | Class           |
| Enum        | Function        |
| Int         | Closure         | 
| Double      |                 |
| String      |                 |
| Set         |                 |
| Tuple       |                 |
| Array       |                 |
| Dictionary  |                 |

https://www.youtube.com/watch?v=wJ1L2nSIV1s

![[Pasted image 20231212110822.png]]

![[Pasted image 20231212110832.png]]

![[Pasted image 20231212110843.png]]

![[Pasted image 20231212111211.png]]

![[Pasted image 20231212111227.png]]