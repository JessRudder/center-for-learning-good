C++ Notes

Operating system looks for main function

int main() {
	// some sort of code
}

Source Code --> run through compiler (g++) --> creates executable
--> executable is what you run with the operating system --> OUTPUT

Minor difference between compilers
  - Cant guarantee code that works on one will work on another
  - Standard code should work on all compilers

Every C++ program is a sequence of declaration (functions, classes, etc)

<type of thing being returned> name_of_function(param1, param2) {
	code to run
}

Integer
	-less than 2^16 guaranteed not to get 'wrap around'
	- e.g. 2^15 + 2^16 should be 2^17 but might wrap to 2^0 and start counting up again
	* Think about this for things like counters (unique ids)
