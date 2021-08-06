
# Golang fundamentals

## Basic types

Golang is a statically typed programming language meaning that each variable has a type, the type is divided into four categories which are as follows:

* Basic type: Numbers, strings, and booleans come under this category.
* Aggregate type: Array and structs come under this category.
* Reference type: Pointers, slices, maps, functions, and channels come under this category.
* Interface type
	
Here, we will discuss Basic Data Types in the Go language. The Basic type category is divided into three subcategories which are:

   * Numbers
   * Booleans
   * Strings


### Numbers

In Go language, numbers are divided into three sub-categories, and those are:

* Integers: In Go language, both signed and unsigned integers are available in four different sizes as shown in the table below. The `signed int` is represented by int type and the `unsigned int` is represented by uint type.
	
		int  int8  int16  int32  int64
		uint uint8 uint16 uint32 uint64 uintptr
		rune // alias for int32, represents a Unicode code point
		byte // alias for uint8
			
		
* Floating-Point Numbers: In Go, floating-point numbers are divided into two categories as shown here:

		float32 float64
		
* Complex Numbers: The complex numbers are divided into two as shown in the table below. The built-in function <i>complex</i> creates a complex number out of real and imaginary parts. And built-in functions <i>real</i> and <i>imag</i> can be used to individually extract real or imaginary parts respectively.
	
		complex64 complex128		
		

### Variables

The `var` statement declares a list of variables;  and as in function argument lists, the type comes last.
A var statement can be used at either package or function levels. We picture both in this example:

```golang
package main

import "fmt"

var c, python, java bool // package level (global)

func main() {
	var i int // function level (local)
	fmt.Println(i, c, python, java)
}
```

Try the code online [here](https://goplay.space/#JDy1N5nZC1K)

### Variables with initializers

A var declaration can include initializers, one per variable. If an initializer is present, the type can be omitted; the variable will take the type of the initializer.

```golang
package main

import "fmt"

var i, j int = 1, 2

func main() {
	var c, python, java = true, false, "no!"  // We are not telling types here!
	fmt.Println(i, j, c, python, java)
}
```

Try the code online [here](https://goplay.space/#Oj4jgArj1uS)

Grouped declaration:

```golang
var (
    home   = os.Getenv("HOME")
    user   = os.Getenv("USER")
    gopath = os.Getenv("GOPATH")
)
```

### Short Variable Declaration Operator := in Go

Short Variable Declaration Operator, :=, in Go is used to create the variables having a proper name and initial value. The main purposes of using this operator are to declare and initialize the local variables inside the functions, and to narrow the scope of the variables.

```golang
package main

import "fmt"

func main() {

	// declaring and initialzing the variable
	a := 30

	// taking a string variable
	Language := "Go Programming"

	fmt.Println("The Value of a is: ", a)
	fmt.Println("The Value of Language is: ", Language)
	
}
```
Try the code online [here](https://goplay.space/#JIr_SbElCTF)

### Operators

Arithmetic: This operators apply to numeric values and yield a result of the same type as the first operand. The four standard arithmetic operators (+, -, *, /) apply to integer, floating-point, and complex types; + also applies to strings. 

The bitwise logical and shift operators apply to integers only.

```
+    sum                    integers, floats, complex values, strings
-    difference             integers, floats, complex values
*    product                integers, floats, complex values
/    quotient               integers, floats, complex values
%    remainder              integers

&    bitwise AND            integers
|    bitwise OR             integers
^    bitwise XOR            integers
&^   bit clear (AND NOT)    integers

<<   left shift             integer << unsigned integer
>>   right shift            integer >> unsigned integer
```

__Comparison operators__: Comparison operators compare two operands and yield an untyped boolean value

```
==    equal
!=    not equal
<     less
<=    less or equal
>     greater
>=    greater or equal
```
__Logical operators__: Apply to boolean values and yield a result of the same type as the operands. The right operand is evaluated conditionally. 

```
&&    conditional AND    p && q  is  "if p then q else false"
||    conditional OR     p || q  is  "if p then true else q"
!     NOT                !p      is  "not p"
```

### Conversions
The expression T(v) converts the value v to the type T.

Some numeric conversions:

```golang
var i int = 42
var f float64 = float64(i)
var u uint = uint(f)

// Or, put more simply: 

i := 42
f := float64(i)
u := uint(f)
```

### Constants

Constants are declared like variables, but with the const keyword.
Constants can be character, string, boolean, or numeric values.
Constants cannot be declared using the := syntax. 
Constants can be grouped in declaration: const(...)

```golang
package main

import "fmt"

const Pi = 3.14

func main() {
	const World = "世界"
	fmt.Println("Hello", World)
	fmt.Println("Happy", Pi, "Day")

	const Truth = true
	fmt.Println("Go rules?", Truth)
}
```

Try the code online [here](https://goplay.space/#7ERm0Wv7Vy8)

### Declaring functions

A function is a block of code that performs a specific task. A function takes an input, performs some operations on the input, and generates an output.
A function can take zero or more input arguments. And also return zero or more output values.
In this example, the function add takes two input parameters of type int.

```golang
package main

import "fmt"

func add(x int, y int) int {
	return x + y
}

func main() {
	fmt.Println(add(42, 13))
}
```

Try the code online [here](https://goplay.space/#dIAgSYqy5vV)

### Parameters Passed By Value

Values of actual parameters are copied to the formal parameters function. Then, any changes to the formal parameters made inside the function are not reflected in the actual parameters of the caller.

```golang
package main
  
import "fmt"
  
// function which modifies
// the value
func modify(Z int) {
	Z = 70
}
  
// Main function
func main() {
	var Z int = 10
	  
	fmt.Printf("Before Function Call, value of Z is = %d", Z)
	 
	// passing by value
	modify(Z)
	  
	fmt.Printf("\nAfter Function Call, value of Z is = %d", Z)
}
```

Try the code online [here](https://goplay.space/#7zsKXN395jl)

### Parameters Passed By Reference

Here, you will use the concept of Pointers. The dereference operator `*` is used to access the value at the variable's address. The address-of operator `&` is used to get the address of a variable of any data type. Both the actual and formal parameters refer to the same memory locations, so any changes made inside the function to the formal parameters are actually reflected in the values of actual parameters of the caller.

```golang
package main
 
import "fmt"
  
// function which modifies
// the value
func modify(Z *int) {
	*Z = 70
}
  
// Main function
func main() {
  
	var Z int = 10
  
	fmt.Printf("Before Function Call, value of Z is = %d", Z)
  
	// paasing by Reference
	// by passing the address
	// of the variable Z
	modify(&Z)
  
	fmt.Printf("\nAfter Function Call, value of Z is = %d", Z)
}
```

Try the code online [here](https://goplay.space/#2AlC75XuZIy)

### What are first class functions?

A language supporting first class functions allows them to be assigned to variables, passed as arguments to other functions and be returned from other functions. Go has support for first class functions.

Anonymous functions: Go language provides a special feature known as an anonymous function. An anonymous function is a function which doesn’t have a name. It is useful when you want to create an inline function.

```golang
package main

import "fmt"

func main() {
	a := func() {
		fmt.Println("hello world first class function")
	}
	a()
	fmt.Printf("%T", a)
}
```

Try the code online [here](https://goplay.space/#hOvyNYD8-S3)

__Higher-Order__: In Go, a function is called a Higher-Order Function if it fulfills one of the following conditions:
	
* Passing Functions as an Argument to Another Function
	
```golang
// Golang program to illustrate how to pass
// a function as an argument to another
// function
package main
  
import (
	"fmt"
	"math"
)
  
// Volume function takes
// a function as a argument
func Sphere(vol func(radius float64) float64) {
  
	fmt.Println("Volume of Sphere is:", vol(3))
}
  
// Main Function
func main() {
  
	volume_of_sphere := func(radius float64) float64 {
		result := 4 / 3 * math.Pi * radius * radius * radius
		return result
	}
  
	// Passing function as an
	// argumnt in Volume function
	Sphere(volume_of_sphere)
}
```
Try the code online [here](https://goplay.space/#M5YGgKiqr5q)

* Returning Functions From Other Functions

```golang
package main
  
import (
	"fmt"
	"math"
)
  
// Here, Volume function returns
// an anonymous function
func Sphere() func(radius float64) float64 {
  
	result := func(radius float64) float64 {
		volume := 4 / 3 * math.Pi * radius * radius * radius
		return volume
	}
  
	return result
}
  
// Main Function
func main() {
  
	sVol := Sphere()
	fmt.Println("Volume of Sphere is:", sVol(5))
}
```

Try the code online [here](https://goplay.space/#tvuyqK3BSX1)

__Variadic function__: A function that is called with a varying number of arguments is known as a variadic function. Or in other words, a user is allowed to pass zero or more arguments into the variadic function

```golang
// Go program to illustrate the
// concept of variadic function
package main
  
import(
	"fmt"
	"strings"
)
  
// Variadic function to join strings
func joinstr(element...string)string{
	return strings.Join(element, "-")
}
  
func main() {
	
  // zero argument
   fmt.Println(joinstr())
	 
   // multiple arguments
   fmt.Println(joinstr("GEEK", "GFG"))
   fmt.Println(joinstr("Geeks", "for", "Geeks"))
   fmt.Println(joinstr("G", "E", "E", "k", "S")) 
}
```

Try the code online [here](https://goplay.space/#Kvp4WmwSffz)

### Closures

Go functions may be closures. A closure is a function value that references variables from outside its body. The function may access and modify the referenced variables; in this sense the function is "bound" to the external variables.
 
```golang
package main

import "fmt"

func adder() func(int) int {
	sum := 0
	return func(x int) int {
		sum += x
		return sum
	}
}

func main() {
	pos, neg := adder(), adder()
	for i := 0; i < 10; i++ {
		fmt.Println(
			pos(i),
			neg(-2*i),
		)
	}
}
```

Try the code online [here](https://goplay.space/#jcGg3dndii2)

### Exported names

In Go, a name is exported if it begins with a capital letter. For example, Pizza is an exported name, as is Pi, which is exported from the math package. pizza and pi do not start with a capital letter, so they are not exported.
When importing a package, you can refer only to its exported names. Any "unexported" names are not accessible from outside the package.

```golang
package main

import (
	"fmt"
	"math"
)

func main() {
	fmt.Println(math.Pi)
}
```

Try the code online [here](https://goplay.space/#Jcp1fMKMbr3)

### Type Assertions

A type assertion takes an interface value and extracts from it a value of the specified explicit type. Basically, it is used to remove the ambiguity from the interface variables.

```
Syntax: t := value.(typeName)
```

```golang
package main

import (
	"fmt"
)
  
// main function
func main() {
	  
	// an interface that has 
	// a string value
	var value interface{} = "GeeksforGeeks"
	  
	// retrieving a value
	// of type string and assigning
	// it to value1 variable
	var value1 string = value.(string)
	  
	// printing the concrete value
	fmt.Println(value1)
	  
	// this will panic as interface
	// does not have int type
	var value2 int = value.(int)
	  
	fmt.Println(value2)
}
```

Try the code online [here](https://goplay.space/#TZVK-sTicRI)
