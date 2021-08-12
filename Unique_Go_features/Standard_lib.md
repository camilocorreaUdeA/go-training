# Go's standard library

Go has a rich and extensive standard library with primitives and built-in functions really useful to solve routinary tasks and by the way avoid bolier-plate code. Packages included in the standard library approach string formating, buffers for I/O, encoders, networking, operating system functionalities, regular expressions, low-level interfaces, testing, mathematical functions, error handling, logging, strings and slice manipulation, reflection, basic I/O primitives, files and path handling, time functions and many others.

Here we will give a brief introduction to the most extended and commonly used, anyways inviting you to explore deeper in the standard library by yourself.

## os
Provides a platform-independent interface to operating system functionality.

This package includes OS functions to deal with system environment. Among others, functions to handle environment variables, system directories and files, OS processes, system permissions, system I/O, etc.

```golang
//os.Args = read command line arguments
package main

import (
	"fmt"
	"os"
)

func main(){
	//Run go run main.go value key=value -f --flag
	fmt.Println("Command line arguments:", os.Args)
}
```

```golang
//os.Exit = kill the current process and exit the program with a status code
package main

import (
	"fmt"
	"time"
	"os"
)

func main(){
	fmt.Println("main started!")
	defer func(){
		fmt.Println("main completed!")
	}
	time.Sleep(time.Duration(rand.Intn(1e3)) * time.Second)
	os.Exit(0)
}
```
```golang
//Environment variables: os.Environ(), os.Getenv(env_var), os.LookupEnv(env_var), os.Setenv(env_var, value), os.Unsetenv(env_var)
package main

import (
	"fmt"
	"os"
)

func main(){
	var env_vars []string = os.Environ()
	fmt.Println(env_vars)
	
	path = os.Getenv("PATH")
	fmt.Println(path)
	
	if goos, found := os.LookupEnv("GOOS"); !found{
		fmt.Println("Environment variable GOOS not found in system!")
	}
	fmt.Printf("GOOS=%s\n", goos)
	
	os.Setenv("GOOS", "linux")
	os.Unsetenv("GOOS")
}
```
```golang
//File handling os.ReadFile(file), os.WriteFile(file, data, permissions)
package main

import (
	"fmt"
	"os"
)

func main() {
	err := os.WriteFile("someDir/someFile", []byte("Hello, Gophers!"), 0666)
	if err != nil {
		fmt.Println(err)
	}
	
	data, err2 := os.ReadFile("someDir/someFile")
	if err2 != nil {
		fmt.Println(err2)
	}
	os.Stdout.Write(data) //Write to standard output
}
```

## fmt
Implements formatted I/O with functions analogous to C's printf and scanf. The format 'verbs' are derived from C's but are simpler.

ftm.Print(s ...interface{}), fmt.Println(s ...interface{}), fmt.Printf(format string, a ...interface{})
fmt.Scan(s ...interface{}), fmt.Scanln(s ...interface{}), fmt.Scanf(format string, a ...interface{})
ftm.Sprint(s ...interface{}), fmt.Sprintln(s ...interface{}), fmt.Sprintf(format string, a ...interface{})

```golang
package main

import (
	"fmt"
)

func main() {
	fmt.Print("Hello Gopher!\n")
	const year, month = 2021, "May"
	fmt.Println("Hello Gopher!", month, year)
	hello, gopher := "Hello", "Gopher"
	fmt.Printf("%s %s\n", hello, gopher)
	
	var name string
	fmt.Scan(&name)
	fmt.Println("Hello", name)
	
	var age int
	fmt.Scanf("%d", &age)
	fmt.Printf("You are %d years old\n", age)
	
	s := fmt.Sprint("Your name is ", name, " and you're ", age, " years old")
	fmt.Println(s)
	
	st := fmt.Sprintf("Your name is %s and you're %d years old", name, age)
	fmt.Println(st)	
}
```
Check it [here](https://goplay.space/#OuhLesl8TCO)

## time
Package time provides functionality for measuring and displaying time. The calendrical calculations always assume a Gregorian calendar, with no leap seconds.

```golang
package main

import (
	"fmt"
	"time"	
)

func reallySlowFunc(){
	time.Sleep(time.Duration(rand.Intn(1e3))) * time.Second)
}

func main() {
    start := time.Now()
    reallySlowFunc()
    reallySlowFunc()
    reallySlowFunc()
    reallySlowFunc()
    reallySlowFunc()
    duration := time.Since(start)
    fmt.Println(duration)
}
```
Check it [here](https://goplay.space/#L-8wsUoVe91)

And...

```golang
package main

import (
	"fmt"
	"time"	
	"sync"
)

func reallySlowFunc(){
	time.Sleep(time.Duration(5) * time.Second)
	wg.Done()
}

var wg sync.WaitGroup

func main() {    
    wg.Add(5)
    start := time.Now()
    go reallySlowFunc()
    go reallySlowFunc()
    go reallySlowFunc()
    go reallySlowFunc()
    go reallySlowFunc()
    wg.Wait()
    duration := time.Since(start)
    fmt.Println(duration)
}
```
Check it [here](https://goplay.space/#hbOGK5ybf2U)

### Dates
```golang
package main

import (
	"fmt"
	"time"
)

func main() {    
	year, month, day := time.Now().Date()
	fmt.Println(year, month, day)
}
```
Check it [here](https://goplay.space/#uvb_wGt2YZR)

## math
Package math provides basic constants and mathematical functions. Includes math/cmplx for complex numbers and math/rand for pseudo-random number generators.

```golang
package main

import (
	"fmt"
	"math"
	"math/cmplx"
)

func main() {    
	radius := 25.14
	area := math.Pi * math.Pow(radius, 2)
	fmt.Println(math.Ceil(area))
	
	conj := cmplx.Conj(2+4i)
	r, theta := cmplx.Polar(conj)
	fmt.Printf("r: %f, theta: %f", r, theta)	
}
```
Check it [here](https://goplay.space/#Tk8whK2hatn)

## strings
Implements simple functions to manipulate UTF-8 encoded strings.

The strings package provides useful functions for basic strings handling and manipulation like comparisons, replacing, joining, splitting, trimming, uppercasing and lowercasing, indexing, basic string search, repeating, counting, etc.

```golang
package main

import (
	"fmt"
	"strings"
)

func main(){
	dog, cat := "dog", "cat"
	fmt.Println(strings.Compare(dog, cat))
	
	if strings.Contains("hot-dogs", dog){
		fmt.Println("Yummm!")
	}
	
	duh := "people popping pop-corn put puppy pet"
	fmt.Println(strings.Count(duh, "p"))
	
	golang := "Golang"
	if strings.HasPrefix(golang, "Go"){
		fmt.Println("Go go go go")
	}
	
	python := "Python is good for data science\nPython is good for web development\nPython is a versatile and easy language\n"
	fmt.Print(strings.ReplaceAll(python, "Python", "Go"))
	
	raw := "This_string_needs_some_processing"
	fmt.Println(strings.ToUpper(strings.Join(strings.Split(raw, "_"), " ")))	
}
```
Check it [here](https://goplay.space/#ELRB1k1o2s0)

## strconv
Implements conversions to and from string representations of basic data types.

This package contains functions for numeric conversions, number to string and viceversa. And also string to string conversions. 

### Numeric conversions
```golang
package main

import (
	"fmt"
	"strconv"
)

func main(){
	snum := "88"
	num, err := strconv.Atoi(snum)
	if err != nil{
		fmt.Println("Conversion failed!")
	}
	fmt.Printf("%s\n", strconv.Itoa(num))
	
	t := "true"
	if s, err := strconv.ParseBool(t); err == nil {
		fmt.Printf("%T, %v\n", s, s)
	}
	
	decimal := "12345.678"
	if ss, err := strconv.ParseFloat(decimal, 64); err == nil {
		fmt.Printf("%T, %v\n", ss, ss)
	}	
}
```
Check it [here](https://goplay.space/#Kyt_W2DQKh2)

### String conversions
```golang
package main

import (
	"fmt"
	"strconv"
)

func main(){
	jsonString := `"{"name":"gopher","age":25,"job":"developer","likes":"concurrency"}"`
	quotedJson := strconv.Quote(jsonString)
	fmt.Println(quotedJson)
}
```
Check it [here](https://goplay.space/#yM2KVAlXsa2)

## sort
Provides primitives for sorting slices and user-defined collections.

```golang
package main

import (
	"fmt"
	"sort"
)

func main(){
	integers := []int{4, 25, 89, 101, 2, 77}
	sort.Ints(integers)
	fmt.Println(integers)
	
	decimals := []float64{101.89, 22.56, 45.23, 0.023}
	sort.Float64s(decimals)
	fmt.Println(decimals)
	
	strns := []string{"house", "golang", "meal", "fruits", "zimbawe", "price"}
	sort.Strings(strns)
	fmt.Println(strns)	
}
```
Check it [here](https://goplay.space/#FSM7noIDv2U)

### Custom comparator: The functions sort.Slice and sort.SliceStable can be used to provide custom comparators.
```golang
package main

import (
	"fmt"
	"sort"
)

func main() {
	people := []struct {
		Name string
		Age  int
	}{
		{"Gopher", 7},
		{"Alice", 55},
		{"Vera", 24},
		{"Bob", 75},
	}
	sort.Slice(people, func(i, j int) bool { return people[i].Name < people[j].Name })
	fmt.Println("By name:", people)

	sort.Slice(people, func(i, j int) bool { return people[i].Age < people[j].Age })
	fmt.Println("By age:", people)
}
```
Check it [here](https://goplay.space/#JNhlfYoZgCV)

## bytes
Implements functions for the manipulation of byte slices. It is analogous to the facilities of the strings package.

```golang
package main

import (
	"bytes"
	"fmt"
	"os"
)

func main() {
	var b bytes.Buffer // A Buffer needs no initialization.
	b.Write([]byte("Hello "))
	fmt.Fprintf(&b, "world!")
	b.WriteTo(os.Stdout)
}
```
Check it [here](https://goplay.space/#b48TYKuEmiA)

```golang
package main

import (
	"bytes"
	"fmt"
)

func main() {
	fmt.Println(bytes.ContainsRune([]byte("I like seafood."), 'f'))
	fmt.Println(bytes.ContainsRune([]byte("I like seafood."), 'ö'))
	fmt.Println(bytes.ContainsRune([]byte("去是伟大的!"), '大'))
	fmt.Println(bytes.ContainsRune([]byte("去是伟大的!"), '!'))
	fmt.Println(bytes.ContainsRune([]byte(""), '@'))
}
```
Check it [here](https://goplay.space/#eHBpeJ-AYzw)

## log
Package log implements a simple logging package. It defines a type, Logger, with methods for formatting output. It also has a predefined 'standard' Logger accessible through helper functions Print[f|ln], Fatal[f|ln], and Panic[f|ln], which are easier to use than creating a Logger manually.

Although, log package works well for logging events that could happen in you app's execution, it is recommended to replace it with a third-party library or logging tool that implemented logging levels. Beware that Fatal level functions in the log package always call os.Exit(1) so after writing the log message they inevitably finish the application, which is not desired in certain situations.

```golang
package main

import (
    "bytes"
	"log"
)

func main() {
    logFile, err := os.OpenFile("app_logger.log", os.O_APPEND|os.O_CREATE|os.O_WRONLY, 0644)
	if err != nil {
		log.Println(err)
	}
	defer logFile.Close()
	
	prefix := "INFO"
	logger := log.New(logFile, prefix, log.LstdFlags)
	logger.Println("info to append")
	logger.Println("more info to append")
}
```

## io and bufio

Package io provides basic interfaces to I/O primitives. Its primary job is to wrap existing implementations of such primitives, such as those in package os, into shared public interfaces that abstract the functionality, plus some other related primitives.

### io
The most prominent features in io package are the io.Reader and io.Writer interfaces. io.Reader reads up to len(p) bytes into internal byte slice, when Reader encounters an error or end-of-file (EOF) condition after successfully reading n > 0 bytes, it returns the number of bytes read.

io. Writer writes len(p) bytes from p to the underlying data stream. It returns the number of bytes written from p (0 <= n <= len(p)) and any error encountered that caused the write to stop early.

```golang
package main

import (
	"bytes"
	"fmt"
	"io"
	"log"
)

func main() {
	bytez := bytes.NewReader([]byte(`Welcome Home`))

	b, err := io.ReadAll(bytez)
	if err != nil {
		log.Fatal(err)
	}

	fmt.Printf("%s", string(b))
}
```
Check it [here](https://goplay.space/#4kyxTrTJ28T)

### bufio
Package bufio implements buffered I/O. It wraps an io.Reader or io.Writer object, creating another object (Reader or Writer) that also implements the interface but provides buffering and some help for textual I/O.

bufio.Writer accumulates data in a buffer before doing a write operation, this way avoiding the CPU overhead caused by many small write operations. With bufio the write operation is only made once the buffer is full or a flush operation is called.

```golang
package main

import (
	"bufio"
	"fmt"
	"os"
)

func main() {
	w := bufio.NewWriterSize(os.Stdout, 5)
	fmt.Fprint(w, "Hello, ")
	fmt.Fprint(w, "world!")
	fmt.Fprint(w, "\nGophers ")
	fmt.Fprint(w, "and ")
	fmt.Fprint(w, " Endavans!")
	fmt.Fprint(w, "\nFlush and ")
	fmt.Fprint(w, " write!")
	w.Flush() //Flushing to force write
}
```
Check it [here](https://goplay.space/#UtZ7BFXEL0O)

## encoding
Package encoding defines interfaces shared by other packages that convert data to and from byte-level and textual representations. Packages that check for these interfaces include encoding/gob, encoding/json, and encoding/xml.

```golang
package main

import (
    "bytes"
    "encoding/json"
	"fmt"
	"log"
)

type Message struct {
    Name string  `json:"name"`
    Body string  `json:"body"`
    Time int64   `json:"time"`
}

func (m Message) String() string{
	return fmt.Sprintf("Name: %s, Body: %s, Time: %d", m.Name, m.Body, m.Time)
}

func main() {
    m := Message{"Alice", "Hello", 1294706395881547000}
	b, err := json.Marshal(m)
	if err != nil{
		log.Println("Error encoding to json byte array")
	}
	if bytes.Compare(b, []byte(`{"name":"Alice","body":"Hello","time":1294706395881547000}`)) == 0{
		log.Println("Success!")
	}
	
	msg := Message{}
	err = json.Unmarshal(b, &msg)
	if err != nil{
		log.Println("Error decoding json")
	}
	log.Println(msg)	
}
```
Check it [here](https://goplay.space/#H95xEl3H4Jw)

## net
Package net provides a portable interface for network I/O, including TCP/IP, UDP, domain name resolution, and Unix domain sockets.

Although the package provides access to low-level networking primitives, most clients will need only the basic interface provided by the Dial, Listen, and Accept functions and the associated Conn and Listener interfaces.

For the purposes of this short bootcamp we only review net/http and net/url packages that include practical and easy-to-use primitives for web development.

### net/url: URL parsing and query escaping.

```golang
package main

import (
	"fmt"
    "net/url"
)

func main() {
    eUrl := "https://www.endava.com/search?developer=gopher&lang=go&range=engineer"
	u, err := url.Parse(eUrl)
	if err != nil {
		fmt.Println(err)
	}
	parsed := fmt.Sprintf("Scheme: %s, Host: %s, Path: %s, Query: %s", u.Scheme, u.Host, u.Path, u.Query())
	fmt.Println(parsed)
		
	queryString := "event=endava fest&date=june 2021&guest=Elon Musk"
	eQuery := url.PathEscape(queryString)
	fmt.Println(eQuery)
	queryParams, _ := url.ParseQuery(eQuery)
	fmt.Println(queryParams)	
}
```
Check it [here](https://goplay.space/#wiCjBsPMAvv)

### net/http: Provides HTTP client and server implementations.

```golang
//Basic http client 1
package main

import (
	"fmt"
	"io"
	"net/http"
)

func main() {
	resp, err := http.Get("https://www.endava.com/en")
	if err != nil {
		log.Fatal(err)
	}

	statusCode := resp.StatusCode
	headers := resp.Header
	body, err := io.ReadAll(resp.Body)
	if err != nil {
		fmt.Println(err)
	}
	defer resp.Body.Close()

	fmt.Println("Status Code:\n", statusCode)
	fmt.Println("Headers:\n", headers)
	fmt.Println("Body:\n", (string(body))	
}
```

```golang
//Basic http client 2
package main

import (
	"fmt"
	"io"
	"log"
	"net/http"
)

type Response struct {
	UserID    int    `json:"userId"`
	ID        int    `json:"id"`
	Title     string `json:"title"`
	Completed bool   `json:"completed"`
}

func main() {
	client := &http.Client{}
	url := "https://jsonplaceholder.typicode.com/todos/1"
	req, err := http.NewRequest(http.MethodGet, url, nil)
	if err != nil {
		log.Fatal(err)
	}

	resp, err := client.Do(req)
	if err != nil {
		log.Fatal(err)
	}

	statusCode := resp.StatusCode
	headers := resp.Header
	body, err := io.ReadAll(resp.Body)
	if err != nil {
		fmt.Println(err)
	}
	defer resp.Body.Close()

	response := &Response{}
	err = json.Unmarshal(body, response)
	if err != nil {
		fmt.Println(err)
	}

	fmt.Println("Status Code:\n", statusCode)
	fmt.Println("Headers:\n", headers)
	fmt.Println("Body:\n", *response)
}
```
```golang
//Basic server
package main

import (
	"encoding/json"
	"io"
	"log"
	"net/http"
	"strconv"
	"strings"
)

type Endavan struct {
	ID    int    `json:"id"`
	Name  string `json:"name"`
	City  string `json:"city"`
	Lang  string `json:"lang"`
	Title string `json:"title"`
	_     struct{}
}

var endavans []Endavan

func endavansCrud(w http.ResponseWriter, r *http.Request) {
	switch r.Method {
	case "GET":
		if len(r.URL.Query()) > 0 {
			queryEndavan(w, r)
			return
		}
		listEndavans(w, r)
		return
	case "POST":
		createEndavan(w, r)
		return
	default:
		w.WriteHeader(http.StatusMethodNotAllowed)
		w.Write([]byte("only get and post so far..."))
	}
}

func queryEndavan(w http.ResponseWriter, r *http.Request) {
	name := r.URL.Query()["name"]
	if len(name) > 0 {
		for _, e := range endavans {
			if e.Name == name[0] {
				jsonObj, err := json.Marshal(e)
				if err != nil {
					w.WriteHeader(http.StatusInternalServerError)
					w.Write([]byte(err.Error()))
					return
				}
				w.WriteHeader(http.StatusOK)
				w.Write(jsonObj)
				return
			}
		}
	}
	w.WriteHeader(http.StatusBadRequest)
	w.Write([]byte("missing query param"))
}

func listEndavans(w http.ResponseWriter, r *http.Request) {
	jsonObj, err := json.Marshal(endavans)
	if err != nil {
		w.WriteHeader(http.StatusInternalServerError)
		w.Write([]byte(err.Error()))
		return
	}
	w.WriteHeader(http.StatusOK)
	w.Write(jsonObj)
}

func createEndavan(w http.ResponseWriter, r *http.Request) {
	reqBody, err := io.ReadAll(r.Body)
	if err != nil {
		w.WriteHeader(http.StatusInternalServerError)
		w.Write([]byte(err.Error()))
		return
	}
	defer r.Body.Close()

	newEndavan := &Endavan{}
	err = json.Unmarshal(reqBody, newEndavan)
	if err != nil {
		w.WriteHeader(http.StatusBadRequest)
		w.Write([]byte(err.Error()))
		return
	}

	endavans = append(endavans, *newEndavan)
	w.WriteHeader(http.StatusCreated)
}

func getEndavan(w http.ResponseWriter, r *http.Request) {
	path := r.URL.Path
	sId := strings.Split(path, "/")[2]
	id, err := strconv.Atoi(sId)
	if err != nil {
		log.Println("Conversion failed!")
		return
	}
	for _, e := range endavans {
		if e.ID == id {
			json.NewEncoder(w).Encode(e)
			return
		}
	}
}

func main() {
	endavans = []Endavan{
		{ID: 1, Name: "Eddie", City: "Cali", Lang: "Go", Title: "Senior Technician"},
		{ID: 2, Name: "Juan", City: "Medellin", Lang: "Go", Title: "Senior Technician"},
		{ID: 3, Name: "Daniela", City: "Medellin", Lang: "Javascript", Title: "Technician"},
		{ID: 4, Name: "Carolina", City: "Bogota", Lang: "Python", Title: "Engineer"},
	}

	http.HandleFunc("/endavans", endavansCrud)
	http.HandleFunc("/endavans/", getEndavan)
	log.Fatal(http.ListenAndServe(":8080", nil))
}
```
## reflect
Package reflect implements run-time reflection, allowing a program to manipulate objects with arbitrary types. The typical use is to take a value with static type interface{} and extract its dynamic type information by calling TypeOf, which returns a Type.

A call to ValueOf returns a Value representing the run-time data. Zero takes a Type and returns a Value representing a zero value for that type.

```golang
package main

import (
    "fmt"
    "reflect"
)

type TestReflection struct {
    field1 int
    field2 string
    field3 float64
}

func main() {

    tf := TestReflection{1, "test", 100.9}

    myType := reflect.TypeOf(tf)
    fmt.Println(myType.Name())     //Type name
    fmt.Println(myType.Kind())     //Name of the relying type
    fmt.Println(myType.NumField()) //Only works with structs, panics with any other type

    fieldInfo := myType.Field(0) //Name of a single field of the type
    fmt.Println(fieldInfo.Name)

    myValue := reflect.ValueOf(tf) //Values of the fields in the type
    fmt.Println(myValue)
}
```
Check it [here](https://goplay.space/#o6t1gWc1Edm)

## Useful readings
* https://pkg.go.dev/std
* https://blog.golang.org/io2010-faq
* https://blog.golang.org/laws-of-reflection
* https://blog.golang.org/gofmt
* https://medium.com/rungo/os-functions-for-everyday-use-cases-ce7272d43d12
* https://www.datadoghq.com/blog/go-logging/
* https://www.loggly.com/use-cases/logging-in-golang-how-to-start/
* https://qvault.io/golang/golang-logging-best-practices/
* https://www.educative.io/edpresso/how-to-read-and-write-with-golang-bufio
* https://gist.github.com/suntong/032173e96247c0411140
* https://ofstack.com/Golang/29055/talk-about-the-confusion-about-golang-io-reading-and-writing.html
* https://blog.golang.org/json
* https://benhoyt.com/writings/go-routing/
* https://subscription.packtpub.com/book/application_development/9781786468666/1/ch01lvl1sec10/routing-in-net-http
* https://medium.com/golangspec/introduction-to-bufio-package-in-golang-ad7d1877f762
* https://golang.cafe/blog/golang-writer-example.html








