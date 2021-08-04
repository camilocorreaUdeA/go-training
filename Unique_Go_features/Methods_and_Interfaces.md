# Golang fundamentals

## Methods

Go does not have classes. However, you can define methods on types.
A method is a function with a special receiver argument.
The receiver appears in its own argument list between the func keyword and the method name.
In this example, the Abs method has a receiver of type Vertex named v. 

```golang
package main

import (
	"fmt"
	"math"
)

type Vertex struct {
	X, Y float64
}

func (v Vertex) Abs() float64 {
	return math.Sqrt(v.X*v.X + v.Y*v.Y)
}

func main() {
	v := Vertex{3, 4}
	fmt.Println(v.Abs())
}
```

Try the code online [here](https://goplay.space/#7sCf37sUyAw)

__NOTE__: A method is just a function with a receiver argument.

Here's Abs written as a regular function with no change in functionality. 

```golang
package main

import (
	"fmt"
	"math"
)

type Vertex struct {
	X, Y float64
}

func Abs(v Vertex) float64 {
	return math.Sqrt(v.X*v.X + v.Y*v.Y)
}

func main() {
	v := Vertex{3, 4}
	fmt.Println(Abs(v))
}
```

Try the code online [here](https://goplay.space/#Iprd2SVkqTy)

### Methods continued

You can declare a method on non-struct types, too.

In this example we see a numeric type MyFloat with an Abs method.

You can only declare a method with a receiver whose type is defined in the same package as the method. You cannot declare a method with a receiver whose type is defined in another package (which includes the built-in types such as int).

```golang
package main

import (
	"fmt"
	"math"
)

type MyFloat float64

func (f MyFloat) Abs() float64 {
	if f < 0 {
		return float64(-f)
	}
	return float64(f)
}

func main() {
	f := MyFloat(-math.Sqrt2)
	fmt.Println(f.Abs())
}
```
Try the code online [here](https://goplay.space/#qZyB41gno-Z)
 
## Value or Pointer receivers

You can declare methods with pointer receivers.

This means the receiver type has the literal syntax *T for some type T

```golang
package main

import (
	"fmt"
	"math"
)

type Vertex struct {
	X, Y float64
}

func (v Vertex) Scale(f float64) {
	v.X = v.X * f
	v.Y = v.Y * f
}

func (v *Vertex) Abs() float64 {
	return math.Sqrt(v.X*v.X + v.Y*v.Y)
}

func main() {
	v := &Vertex{3, 4}
	fmt.Printf("Before scaling: %+v, Abs: %v\n", v, v.Abs())
	v.Scale(5)
	fmt.Printf("After scaling: %+v, Abs: %v\n", v, v.Abs())
}
```
Try the code online [here](https://goplay.space/#4xYNZ8fSJXZ)

__NOTE__: There are two reasons to use a pointer receiver.
The first is so that the method can modify the value that its receiver points to.
The second is to avoid copying the value on each method call. This can be more efficient if the receiver is a large struct.

## Interfaces

An interface type is defined as a set of method signatures. 

```golang
package main

import (
	"fmt"
	"math"
)

type Abser interface {
	Abs() float64
}
```

```golang
package main

import "fmt"

type I interface {
	M()
}

type T struct {
	S string
}

// This method means type T implements the interface I,
// but we don't need to explicitly declare that it does so.
func (t T) M() {
	fmt.Println(t.S)
}

func main() {
	var i I = T{"hello"}
	i.M()
}
```
Try the code online [here](https://goplay.space/#MeA6hRVdZ7_k)

__NOTE__: A type implements an interface by implementing its methods. There is no explicit declaration of intent, no "implements" keyword.

### Stringers

One of the most ubiquitous interfaces is Stringer defined by the fmt package. 

```golang
type Stringer interface {
    String() string
}
```

A Stringer is a type that can describe itself as a string. The fmt package (and many others) look for this interface to print values.

```golang
package main

import "fmt"

type Person struct {
	Name string
	Age  int
}

func (p Person) String() string {
	return fmt.Sprintf("%v (%v years)", p.Name, p.Age)
}

func main() {
	a := Person{"Arthur Dent", 42}
	z := Person{"Zaphod Beeblebrox", 9001}
	fmt.Println(a, z)
}
```

Try the code online [here](https://goplay.space/#kjOw0ThGyt3)

### Errors

Go programs express error state with error values.

The error type is a built-in interface similar to fmt.Stringer

```golang
type error interface {
    Error() string
}
```

__NOTE__: As with fmt.Stringer, the fmt package looks for the error interface when printing values.

```golang
package main

import (
	"fmt"
	"time"
)

type MyError struct {
	When time.Time
	What string
}

func (e *MyError) Error() string {
	return fmt.Sprintf("at %v, %s",
		e.When, e.What)
}

func run() error {
	return &MyError{
		time.Now(),
		"it didn't work",
	}
}

func main() {
	if err := run(); err != nil {
		fmt.Println(err)
	}
}
```

Try the code online [here](https://goplay.space/#RQYqhjXVByX)

## The empty interface

The interface type that specifies zero methods is known as the empty interface:

```golang
interface{}
```

An empty interface may hold values of any type. (Every type implements at least zero methods.)

Empty interfaces are used by code that handles values of unknown type. For example, fmt.Print takes any number of arguments of type interface{}.

```golang
package main

import "fmt"

func main() {
	var i interface{}
	describe(i)

	i = 42
	describe(i)

	i = "hello"
	describe(i)
}

func describe(i interface{}) {
	fmt.Printf("(%v, %T)\n", i, i)
}
```

Try the code online [here](https://goplay.space/#CozDz_Qs22A)

## Dependency injection

It is a standard technique for producing flexible and loosely coupled code, by explicitly providing components with all of the dependencies they need to work. In Go, this often takes the form of passing dependencies to constructors

```golang
// myFunc uses an SDK service client to make a request to
    func myFunc(svc dynamodbiface.DynamoDBAPI) bool {
        // Make svc.BatchGetItem request
    }

    func main() {
        sess := session.New()
        svc := dynamodb.New(sess)

        myFunc(svc)
    }

	// In your _test.go file:
    // Define a mock struct to be used in your unit tests of myFunc.
    type mockDynamoDBClient struct {
        dynamodbiface.DynamoDBAPI
    }

    func (m *mockDynamoDBClient) BatchGetItem(input *dynamodb.BatchGetItemInput) (*dynamodb.BatchGetItemOutput, error) {
        // mock response/functionality
    }

    func TestMyFunc(t *testing.T) {
        // Setup Test
        mockSvc := &mockDynamoDBClient{}

        myfunc(mockSvc)

        // Verify myFunc's functionality
    }
```