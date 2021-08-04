# Flow control and built-in types in Go

## Cycles

### Classic loop

```golang
for [ Initial Statement ] ; [ Condition ] ; [ Post Statement ] {
    [Action]
}
```

```golang
for i := 0; i < N; i++{
	//do something
}
```
```golang
counter := 0
for ; counter < N; counter++{
	//do something
}
```
```golang
//Multiple variables in the same loop
for i, j := 0, 1; i < 10; i, j = i+1, j+1 {
	fmt.Println(i, j)
}
```
### Go style while loop

```golang
cond := N
for cond > 0{
	//do something
	cond--;
}
```
### Unknown number of iterations (infinite loop)

```golang
for{
	if someCondition {
        break
    }
    if otherCondition{
        continue
    }
    // do something
}
```
Or
```golang
for i := 0; ; i++{
	if someCondition {
        break
    }
    if otherCondition{
        continue
    }
    // do something
}
```
### Iterating over the elements of a sequential data type (arrays, slices, maps, buffered channels)

```golang
for i, element := range array{
	fmt.Printf("Element %v, is at position %d in array\n", element, i)
}

for key, value := range map{
	fmt.Println("Key", key, "associated to value", value)
}

for element := range channel{
    fmt.Println(element)  //Make sure to close the channel elsewhere, otherwise your app will fall in a deadlock!
}
```
Variables associated to the for clause can be ommited using the underscore (_) character

```golang
for _, element := range slice{
	fmt.Print(element)
}
```
Be careful using ranged loops because they only return copies of the values in the sequence, then modifications will not have any effect unless they are made using the brackets operator and the index for the position of the element. Example:

```golang
package main

import "fmt"

func main(){
    var arr [5]int
    for i, element := range arr{
        element = i
    }
    for _, element := range arr{
        fmt.Print(element)
    }
}
```
[Here](https://goplay.space/#MevPSAVho2M)

Anything wrong? Check this:

```golang
package main

import "fmt"

func main(){
    var arr [5]int
    for i, _ := range arr{
        arr[i] = i
    }
    for _, element := range arr{
        fmt.Print(element)
    }
}
```
[Here](https://goplay.space/#Wx049e4jtvl)

## Conditionals

### if, else if, else

```golang
if [ condition ] {
    [Action]
}else if [ alternative condition ] {
    [Action]
}else{
    [Action]
} 
```
### Assigment and evaluation in place

```golang
if value := rand.Intn(100); value > 60{
	//do something
}
```
### switch, case, default

```golang
switch [ option ] {
	case x: 
	    [ actions ]
	case y:
	    [ actions ]
	case z: 
	    [ actions ]
	default:
	    [ actions ]
}
```
### Classic switch case 

```golang
now := time.Now().Unix()
mins := now % 2
switch mins {
	case 0:
		fmt.Println("even")
	case 1:
		fmt.Println("odd")
}
```
Check it [here](https://goplay.space/#U1cioPEfW7C)

### Multiple cases for the same action

```golang
score := 7
switch score {
	case 0, 1, 3:
		fmt.Println("Terrible")
	case 4, 5:
		fmt.Println("Mediocre")
	case 6, 7:
		fmt.Println("Not bad")
	case 8, 9:
		fmt.Println("Almost perfect")
	case 10:
		fmt.Println("hmm did you cheat?")
	default:
		fmt.Println(score, " off the chart")
}
```
Check it [here](https://goplay.space/#w9BeHlorVgL)

### Falltrough keyword

```golang
flavors := []string{"chocolate", "vanilla", "strawberry", "banana"}

for _, flavor := range flavors {
	switch flavor {
	case "strawberry":
		fmt.Println(flavor, "is my favorite!")
		fallthrough
	case "vanilla", "chocolate":
		fmt.Println(flavor, "is great!")
	default:
		fmt.Println("I've never tried", flavor, "before")
	}
}
```
Check it [here](https://goplay.space/#9Sstbcq539e)

### Optionless switch

```golang
t := time.Now().Hour()
switch {
case t < 12:
	fmt.Println("It's before noon")
default:
	fmt.Println("It's after noon")
}
```
Check it [here](https://goplay.space/#5McUnmP0HPd)

## Defer

The defer keyword in Go pushes a function call as the first element to the current function's stack. So according to the LIFO behaviour of th function stack, the function pushed using the defer statement will be called before the current function execution is finished.

The defer keyword is generally used to clean up resources (close open files, shut down network connections, disconnet a database, etc.)

```golang
package main

import "fmt"

func greet(){
	defer fmt.Println("Bye")
    fmt.Println("Hi")
}

func main() {
    defer fmt.Println("one")
    defer fmt.Println("two")
    defer fmt.Println("three")
}
```
Check it [here](https://goplay.space/#3Hs71WbuVW2)

Recommendations using defer:
<ol>
<li>If you are going to use defer to release any resource: Place defer always after error checking the resource allocation</li>
<li>It is highly recommended that the function that uses the defer returned an error to error-check the execution of the defer function call</li>
</ol>

Example:

```golang
func write(fileName string, text string) error {
    file, err := os.Create(fileName) //Allocating resource
    if err != nil {  //error checking allocation
        return err
    }
    defer file.Close() //Placing defer after error checks
    _, err = io.WriteString(file, text)
    if err != nil {
        return err
    }

    return file.Close() //returning the result of the clean up process
}
```
## Arrays and slices

An array is a numbered sequence of elements of a single type. The number of elements is the length of the array and is never negative.
Array types are always one-dimensional but may be composed to form multi-dimensional types.

The elements can be addressed by integer indices 0 through len(a)-1, where <b>len</b> is a built-in function that returns the length of the array.

The length of the array is encoded in its type, then <b>[4]int</b> results in a type different to <b>[5]int</b>.
Array types are always one-dimensional but may be composed to form multi-dimensional types.

```golang
//Array decalaration and initialization
greetings := [4]string{"Good morning", "Bon jour", "Guten tag", "Buon giorno"}

var nums [5]int
for i := 0; i < len(nums); i++{
	nums[i] = rand.Intn(200)
}
```
Implicit length deduction using ellipsis operator (only valid for short declaration)

```golang
arr := [...]string{"hello", "world"} //Go will infer a fixed length of two
```
The slice type allows us to have collections of variable size (can be either shrinked or enlarged) given that Go slices do not encode the size of the colllection in the type. A slice can be considered a dynamic array.

A slice is also a descriptor for a contiguous segment of an underlying array, then any modifications to the elements of the slice will result in the elements of the underlying array being modified as well.

```golang
package main

import "fmt"

func main() {
    greetings := [4]string{"Good morning", "Bon jour", "Guten tag", "Buon giorno"}
    greets := greetings[0:2] //{"Good morning", "Bon jour"} slicing from array

    greets = append(greets, "Buen dia")  //Appending new elements to the slice
    fmt.Println(len(greets))
    greets[1] = "God morgon"  //Underlying array gets modified as well
    fmt.Println(greetings[1])
}
```
Check it [here](https://goplay.space/#BSMOSb3ITgq)

A slice literal creates the underlying array in the slice declaration

```golang
powers := []int{1, 4, 9, 16, 25, 36, 49}
```

Slice literals can be declared using the keyword make, by specifying both the element type and the initial length for the slice (and optionally an initial capacity)

```golang
mySlice := make([]int, 5) // intialized to [0,0,0,0,0]
```

Built-in functions for arrays and slices

```golang
len(arr) //Number of elements in the array (slice)
operator [pos] //Access the element at the specified position in the array (slice). 
operator [lower_bound:upper_bound] //Slicing a range of elements from array (slice)
append(slice, element) //Appends a new element to the slice
cap(slice) //Check the current capacity of the slice
```
## Maps

A map is an unordered group of elements of one type, called the element type, indexed by a set of unique keys of another type, called the key type. The value of an uninitialized map is nil.

```golang
map[key_type]value_type

var statusCode map[int]string 

statusCode = map[int]string{200:"StatusOK", 201:"StatusCreated", 400:"StatusBadRequest", 404:"StatusNotFound", 500:"StatusInternalServerError"}

queryParams := map[string]string{"brand":"Apple", "max_price":"4500", "min_rating":"3.9", "category":"consumer_electronics", "retailer":"amazon"}
```
Map literals can be created using the keyword make, by specifying map key and value types (and optionally an initial capacity)

```golang
pathParams := make(map[string]string)
```
Adding a key-value pair to a map. Keep in mind that key-value pairs cannot be added to an unitialized map, otherwise your program will panic (panic: assignment to entry in nil map)

```golang
pathParams["id"] = "1234567890" 
```
Check if a key-value pair exists in a map:

```golang
value, found := statusCode[300]
if !found{
	//not found actions
}
```
Or, more Go alike

```golang
if value, found := statusCode[300]; !found{
	//not found actions
}
```
Changing the value of an existing key in a map

```golang
queryParams["retailer"] = "mercado_libre"
```
Removing a key-value pair

```golang
delete(queryParams, "min_rating")
```
Built-in functions for maps

```golang
operator [key] //Acces the value associated to the specified key
delete(map, key) //Remove the key-value pair from the map
len(map) //Number of elements in the map
```
### Double-check when you use built-in data types

Be careful with the scope of initialization. Check this:

```golang
func changeMap(m map[string]int){    
	m["dos"] = 2
}
        
func main() {
   m1 := make(map[string]int)
   m1["dos"] = 4
   changeMap(m1)
   fmt.Println(m1["dos"])
}
```
Check it [here](https://goplay.space/#hijwAROx--X)

And this:

```golang
func changeMap(m map[string]int){
    m = make(map[string]int)    
	m["dos"] = 2
}
        
func main() {
   var m1 map[string]int
   changeMap(m1)
   fmt.Println(m1["dos"])
}
```
Check it [here](https://goplay.space/#3JbZpLaIiV3)

Be careful passing slices as parameters in functions. Check this:

```golang
func changeSlice(s []int){
	s = append(s, 25)
}
        
func main() {
   s := []int{1,2,3,4}
   changeSlice(s)
   fmt.Println(s[4])
}
```
Check it [here](https://goplay.space/#4HDoboU4f8_2)

Any ideas to solve this situation?

## Structs

Structs let you build user-defined types in Go, similar to classes in OOP languages. In general, a struct is a sequence of named elements, called fields, that have a name and a type. Field names may be specified explicitly (IdentifierList) or implicitly (EmbeddedField).

An embedded field is declared within the struct with a type but no explicit name, this is a common practice to inherit other structs fields through composition.

Declaring a struct:

```golang
type struct_identifier struct{
	//struct fields
	x int
	y float64
	s []string
	T struct_type
}
```
Unkeyed fields can be used as padding to define the memory layout of the struct. This can be useful in cases where you need to align subsequent fields to certain size for memory positions that matches the memory layout of the data coming/going to another system.

```golang
type struct_identifier struct{
	x    int
	y    float64
	s    []string
	T    struct_type
	_    [5]int  //unkeyed field
}
```
An unkeyed struct field of struct type can be used to force using the fields' keys in the struct declaration

```golang
type SomeType struct {
  Field1 string
  Field2 bool
  _      struct{}  // unkeyed struct type field
}

// ALLOWED:
bar := SomeType{Field1: "hello", Field2: true}

// COMPILE ERROR:
foo := SomeType{"hello", true}
```
A struct type can have function type fields

```golang
func customFunc() int{
	return 1
}

type customStruct struct{
	f func() int
}

func main(){
	st := customStruct{f:customFunc}
	fmt.Println(st.f())
}
```
Check it [here](https://goplay.space/#Gavz4oKQ28c)

Later, we'll see that this can be implemented in a cleaner and more practical way by using function receivers. Adding methods to structs we mean.

### Field tags

A field declaration may be followed by an optional string literal tag, which becomes an attribute for all the fields in the corresponding field declaration. An empty tag string is equivalent to an absent tag.

Struct tags are annotations with small pieces of metadata that provide instructions and information to other packages that interact with the struct.

A tag is a short string enclosed withing a pair of tilde characters

```golang
type User struct {
    Name          string    `json:"name"`
    Password      string    `json:"password"`
    PreferredFood []string  `json:"preferredFood"`
    CreatedAt     time.Time `json:"createdAt"`
	ModifiedAt    time.Time `json:"modifiedAt"`
}
```
### Struct composition

Go is not an object-oriented language or at least not in an explicit way. Therefore, composition is the only way to implement some sort of inheritance between Go structs. Composition is implemented by including an embedded struct field in the heiress struct.

```golang
type Employee struct{
	       User
	Salary float64
}

func main(){
    //One way
	john := Employee{User:User{"John", "qwerty", []string{"pasta", "tacos"}, time.Now(), time.Now()}, Salary:3500}
	
	//Another way
	jane := Employee{}
	jane.Name = "Jane"
	jane.Password = "p455w0rd"
	jane.PreferredFood = []string{"vegetables", "fruits"}
	jane.CreatedAt = time.Now()
	jane.ModifiedAt = time.Now()

	fmt.Println(john)
	fmt.Println(jane)
}
```
Check it [here](https://goplay.space/#-xZVsjCMHWR)

## Pointers

Pointers are variables that can store memory addresses. When a pointer points to a variable, you can access directly to the memory location of that variable. The latter is quite useful when you need to alter the value of data that you are passing to a function or when you need to access the information in a large data structure but you don't want to make an unnecessary copy of such amount of information.

You will definitely need pointers in the following situations:
<ul>
<li>Pass parameters by reference in functions</li>
<li>Avoid unnecessary copies of large data structures passed as function parameters</li>
<li>Pointer receivers: let methods modify struct fields</li>
</ul>

Pointers are declared as any other variable but the type needs to be prefixed with a star character (*) 

```golang
var pointer *int32  //A pointer to int32 variables
```
Pointers can be pointed to variables using the & operator

```golang
myVar := 25
pointer = &myVar
```
Short declaration is also valid for pointers

```golang
myVar := 25
pointee := &myVar
```
Pointers to structs can be declared using the keyword new

```golang
structPtr := new(CustomStructType) // This is equivalent to structPtr := &CustomStructType{}
```
To access the value allocated in the memory address stored in the pointer you have to use the star operator (*)

```golang
*pointee = 50
fmt.Println(*pointee)
```
Unlike C or C++, Go pointers do not have pointer arithmetics, so you're not allowed to surf nearby memory locations using a pointer.

### Passing by reference

```golang
func foo(val *int){
	*val += 10
}

func main(){
	bar := 8
	ptr := &bar
	foo(&bar)
	fmt.Println(bar)
	foo(ptr)
	fmt.Println(bar)
}
```
Check it [here](https://goplay.space/#_6iRhHsDNIp)

With structs

```golang
type User struct {
    Name          string    `json:"name"`
    Password      string    `json:"password"`    
}

func cleanUserData(user *User){
		user.Name = ""
		user.Password = ""
}

func main(){
	john := User{"john", "qwerty"}
	cleanUserData(&john)
	fmt.Printf("%v", john)
}
```
Check it [here](https://goplay.space/#kl6TBXp4M_V)

## make and new keywords

### Make

This keyword is used to initialize the memory allocated to slices, maps and channels. Make also allocates memory for lenght and capacity of the built-in type being initialized.

```golang
customSlice := make([]myStruct, 10, 25) //zero-valued myStruct elements in the slice
customMap := make(map[string]int, 10) //10 maps initialized with nil value
customChannel := make(chan int) //Unbuffered channel initialized with nil
customBufCahnnel := make(chan int, 10)  //Buffered channel initialized with nil (empty buffered channel)
```
### New

This keyword is used to initialize with a zero-value the memory allocated to a pointer variable, one of type *T. This is useful for preventing unsafe dereferenciation of uninitialized pointers.

```golang
var bar *int //Cannot be safely dereferenced, only after correct initialization
var foo = new(int) //Initialized, *foo = 0
```

## Useful readings

* https://go101.org/article/blocks-and-scopes.html
* https://golang.org/ref/spec
* https://www.digitalocean.com/community/tutorial_series/how-to-code-in-go
* https://yourbasic.org/golang/
* https://dave.cheney.net/2014/08/17/go-has-both-make-and-new-functions-what-gives
* https://stackoverflow.com/questions/49428716/pass-slice-as-function-argument-and-modify-the-original-slice
* https://www.digitalocean.com/community/tutorials/how-to-use-struct-tags-in-go-es
* https://www.ardanlabs.com/blog/2013/08/understanding-slices-in-go-programming.html
* https://golangbot.com/arrays-and-slices/
* https://golangbot.com/structs/
