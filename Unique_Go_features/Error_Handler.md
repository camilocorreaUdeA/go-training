# Error handling and Go

If you have written any Go code you have probably encountered the built-in error type. Go code uses error values to indicate an abnormal state. For example, the os.Open function returns a non-nil error value when it fails to open a file.

```golang
func Open(name string) (file *File, err error)
```

The following code uses os.Open to open a file. If an error occurs it calls log.Fatal to print the error message and stop.

```golang
f, err := os.Open("filename.ext")
if err != nil {
    log.Fatal(err)
}
// do something with the open *File f
```

## The error type

The error type is an interface type. An error variable represents any value that can describe itself as a string. Here is the interface's declaration:

```golang
type error interface {
    Error() string
}
```

The most commonly-used error implementation is the errors package's unexported errorString type.

```golang
// errorString is a trivial implementation of error.
type errorString struct {
    s string
}

func (e *errorString) Error() string {
    return e.s
}
```

You can construct one of these values with the errors.New function. It takes a string that it converts to an errors.errorString and returns as an error value.

```golang
// New returns an error that formats as the given text.
func New(text string) error {
    return &errorString{text}
}
```

Here's how you might use errors.New:
```golang
func Sqrt(f float64) (float64, error) {
    if f < 0 {
        return 0, errors.New("math: square root of negative number")
    }
    // implementation
}
```

A caller passing a negative argument to Sqrt receives a non-nil error value (whose concrete representation is an errors.errorString value). The caller can access the error string ("math: square root of...") by calling the error's Error method, or by just printing it:
```golang
f, err := Sqrt(-1)
if err != nil {
    fmt.Println(err)
}
```
__NOTE__: The fmt package formats an error value by calling its Error() string method.

In many cases fmt.Errorf is good enough, but since error is an interface, you can use arbitrary data structures as error values, to allow callers to inspect the details of the error.

As another example, the json package specifies a SyntaxError type that the json.Decode function returns when it encounters a syntax error parsing a JSON blob.
```golang
type SyntaxError struct {
    msg    string // description of error
    Offset int64  // error occurred after reading Offset bytes
}

func (e *SyntaxError) Error() string { return e.msg }
```

The Offset field isn't even shown in the default formatting of the error, but callers can use it to add file and line information to their error messages:
```golang
if err := dec.Decode(&val); err != nil {
    if serr, ok := err.(*json.SyntaxError); ok {
        line, col := findLine(f, serr.Offset)
        return fmt.Errorf("%s:%d:%d: %v", f.Name(), line, col, err)
    }
    
    return err
}
```

## Panics

It is a built-in function that stops the ordinary flow of control and begins panicking. When the function F calls panic, execution of F stops, any deferred functions in F are executed normally, and then F returns to its caller. To the caller, F then behaves like a call to panic. The process continues up the stack until all functions in the current goroutine have returned, at which point the program crashes. Panics can be initiated by invoking panic directly. They can also be caused by runtime errors, such as out-of-bounds array accesses

```golang
func panic(v interface{})   
```

### Runtime Error Panic

Runtime error in the program can happen in below cases

* Out of bounds array access
* Calling a function on a nil pointer
* Sending on a closed channel
* Incorrect type assertion

```golang
package main

import "fmt"

func main() {

	a := []string{"a", "b"}
	print(a, 2)
}

func print(a []string, index int) {
	fmt.Println(a[index])
}
```

Try the code online [here](https://goplay.space/#P8BWVIU4dpZ)

### Calling the panic function explicitly

Some cases where panic function can be called explicitly by the programmer are:

* The function expected a valid argument but instead, a nil argument was supplied. In such a case, the program cannot continue and it will raise a panic for a nil argument passed.

* Any other scenario in which the program cannot continue.

```golang
package main

import "fmt"

func main() {

	a := []string{"a", "b"}
	checkAndPrint(a, 2)
}

func checkAndPrint(a []string, index int) {
	if index > (len(a) - 1) {
		panic("Out of bound access for slice")
	}
	
	fmt.Println(a[index])
}
```

Try the code online [here](https://goplay.space/#jXDhmu0x5oj)

## Recover

It is a built-in function that regains control of a panicking goroutine. Recover is only useful inside deferred functions. During normal execution, a call to recover will return nil and have no other effect. If the current goroutine is panicking, a call to recover will capture the value given to panic and resume normal execution.

```golang
package main

import "fmt"

func main() {
    f()
    fmt.Println("Returned normally from f.")
}

func f() {
    defer func() {
        if r := recover(); r != nil {
            fmt.Println("Recovered in f", r)
        }
    }()
    fmt.Println("Calling g.")
    g(0)
    fmt.Println("Returned normally from g.")
}

func g(i int) {
    if i > 3 {
        fmt.Println("Panicking!")
        panic(fmt.Sprintf("%v", i))
    }
    defer fmt.Println("Defer in g", i)
    fmt.Println("Printing in g", i)
    g(i + 1)
}
```

Try the code online [here](https://goplay.space/#Ujf1dRatTMb)

Another example
```golang
var err interface{}

	invoke := func() {
		defer func() {
			if r := recover(); r != nil {
				err = r
			}
		}()

		_ = NewLogger("")
	}

	invoke()
	fmt.Println("Hiya", err)
```