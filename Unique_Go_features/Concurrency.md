# Concurrency in Go

Concurrency is defined as the capability to manage multiple tasks at once. Concurrency is about handling, not doing, several different tasks at the same time. That means that the application is working to manage the completion of numerous things all at once in a given period of time. However, albeit tasks seem to be running at once there is only one single task being executed at a certain point in time.

Concurrency in Go is handled using goroutines, channels and the sync package.

## Goroutines

Goroutines are functions capable of running concurrently (not in parallel!) with other functions in a Go application. These goroutines can be thought of as light-weight threads, but they are not threads, they are extremely cheap in terms of memory and processing compared to threads, they are not matched to real OS threads and in fact there can be thousands of goroutines sharing the same OS thread at the same time.

Goroutines have their own memory stack that is in general not large, usually  just a few kbytes, that's the reason for a single thread to host many goroutines at once. Besides, goroutines have their own communication mechanism, the channels, that support the philosophy of sharing memory by communicating instead of locking resources or having a signaling system to explicitly handle the state of shared resources (commnicating by sharing memory).

The execution of an application that includes goroutines is asynchronous because goroutines are not waited until they finish, and they do not wait other goroutines to be finished. In fact, the main function of any Go application is a goroutine itself, and will not wait for other goroutines to finish execution unless explicitly told to do so.

When a goroutine is created it is treated as an independent unit of work that is scheduled and executed on an available time slot in the processor. The Go runtime scheduler has features to manage all the goroutines that are created and need processor time. The Go runtime is also responsible for multiplexing gorotuines in the OS thread and moving goroutines to a new thread when the current one has been blocked by a goroutine.

Goroutines are easily created by just prefixing functions calls with the word <b>go</b>

```golang
sum(3, 4) //Regular function
go sum(3, 4) //goroutine
```
In the following code snippet you can see the main function does not wait for the complete execution of the hello goroutine:

```golang
package main

import (  
    "fmt"
)

func hello() {  
    fmt.Println("Hello world goroutine")
}
func main() {  
    go hello()
    fmt.Println("main function")
}
```
Check it [here](https://goplay.space/#3ejkzT4JQbv)

You need to tell the main function to wait (we'll later see more elegant and idiomatic ways to make a goroutine await):

```golang
package main

import (  
    "fmt"
    "time"
)

func hello() {  
    fmt.Println("Hello world goroutine")
}

func main() {  
    go hello()
    time.Sleep(1 * time.Second)
    fmt.Println("main function")
}
```
Check it [here](https://goplay.space/#Vz1SGksWf3X)

## Share memory by communicating - Communicating Sequential Processes

CSP is a high-level concurrency model (introduced in 1978 by Tony Hoare) that combines the sequential thread (threaded state) abstraction with synchronous message passing communication. The message passing primitive is a blocking queue that blocks both on send and receive, meaning that the concurrent computations perform a rendezvous.

Go's concurrency primitives - goroutines and channels - provide an elegant and distinct means of structuring concurrent software. Instead of explicitly using locks to mediate access to shared data, Go encourages the use of channels to pass references to data between goroutines. This approach ensures that only one goroutine has access to the data at a given time.

## Channels

A channel is a typed communication conduit to send and receive messages. Channels can be thought of as pipes that goroutines use to communicate. Go channels prevent data races given that send and receive operations are blocking by default, that suggests that there should be a synchronization between sending and receiven goroutines to effectively communicate messages through the channel and such synchronization prevents goroutines from using explicit locks or conditional variables commonly used in concurrent thread-driven
environments.

So far, channels are useful for communicating goroutines, avoiding data races and for synchronicity as well.

There are two kinds of channels in Go, unbuffered and buffered channels. The former can store a single element while the latter can store multiple elements. 

```golang
uch := make(chan int) //Unbuffered channel
bch := make(chan int, 5) //Buffered channel
```
Operator arrow (<-) is used to access channels in Go, we speak of a sending-on channel if the arrow points to the channel, and a receiving-from channel when the channel is in the tail of the arrow.

```golang
myChan <- 10 //Sending-on channel
myVar := <- myChan //Receiving-from channel
```
A sending-on channel is the one that passes a message to another object, either clearing the channel (if unbuffered) or removing an element from the buffer. While, a receiving-from channel is the one that expects a message to pass it later, filling the channel (in either case buffered or unbuffered) or pushing an element to the buffer.

<ul>
<li>A sending-on channel blocks until a receiving-from channel is ready</li>
<li>A receiving-from channel blocks until there is data in the sending-on channel</li>
</ul>

The aforementioned is not applicable to buffered channels, they only block when sending on a full channel or receiving from an empty channel.

Channels can be closed when it is needed to explicitly tell that the channel will not receive any other message. Go has the <b>close</b> ketyword for this matter.

Trying to receive from a closed channel results in 0 and false (0, false), meaning no available data and channel not working. While sending on a closed channel results in panic.

### Unbuffered channel blockings

Sending-on channel blocking case

```golang
func main(){
	c := make(chan int)
	c <- 5  //No receiving-from channel ready to receive data in channel c
}
```
Check it [here](https://goplay.space/#cVsULkt1F7t)

Receiving-from channel blocking case

```golang
func main(){
	c := make(chan int)
	fmt.Println(<-c) //There's currently no data in channel
}
```
Check it [here](https://goplay.space/#J4qwWWHecw7)

### Buffered channel blockings

Empty buffered channel blocking case

```golang
func main() {
   ch := make(chan int, 5)
   message := <- ch
   fmt.Println(message)
}
```
Check it [here](https://goplay.space/#Qmv8rVCSIVe)

Full buffered channel blocking case

```golang
func main() {
   ch := make(chan int, 5)
   ch <- 1
   ch <- 2
   ch <- 3
   ch <- 4
   ch <- 5
   ch <- 6
   fmt.Println("Hello World")
}
```
Check it [here](https://goplay.space/#e2cM3Mzbpu9)

Refactoring our previous goroutines example to remove that ugly sleep time and use channels instead

```golang
package main

import (  
    "fmt"
)

var ch chan int

func hello() {  
    fmt.Println("Hello world goroutine")
	ch <- 0
}

func main() { 
    ch = make(chan int) 
    go hello()
	<- ch
    fmt.Println("main function")
}
```
Check it [here](https://goplay.space/#_S6MENGmzm0)

A pretty basic example combining goroutines and channels

```golang
package main

import "fmt"

func sum(s []int, c chan int) {
    sum := 0
    for _, v := range s {
        sum += v
    }
    c <- sum 
}

func main() {
    s := []int{7, 2, 8, -9, 4, 0}

    c := make(chan int)
    go sum(s[:len(s)/2], c)
    go sum(s[len(s)/2:], c)
    x, y := <-c, <-c 

    fmt.Println(x, y, x+y)
}
```
Check it [here](https://goplay.space/#44T4Fvd0-_u)

A nice example of goroutines commnicating through channels

```golang
type Ball struct{hits int}

func player(name string, table chan *Ball){
	for{
		ball := <-table //Blocks until there is data in table
		ball.hits++
		fmt.Println(name, ball.hits)
		time.Sleep(100 * time.Millisecond)
		table<-ball //Puts data in the channel, so other Goroutines expecting such data do not fall in a block
	}
}

func main(){
	table := make(chan *Ball)
	go player("ping", table)
	go player("pong", table)
	
	table<-new(Ball) //Blocks main goroutine until there is a receiver ready to get the pointer to ball data
	time.Sleep(1 * time.Second) //Main function waits for a while for Goroutines to do their job
	<-table //This ends the program by blocking the player Goroutines and finishing the main function
}
```
Check it [here](https://goplay.space/#rAOPos5TpV-)

Hands on practice now: Fix the following code to make it work, there is more than one fix.

```golang
package main

import (
    "fmt"
)

func ping(pings chan<- string, msg string) {
    pings <- msg
} 

func pong(pings <-chan string, pongs chan<- string) {
    msg := <-pings
    pongs <- msg
} 

func main() {
    pings := make(chan string)
    pongs := make(chan string)    
    ping(pings, "passed message")
    pong(pings, pongs)
    fmt.Println(<-pongs)
}
```
## Deadlocks

Deadlocks make a Go application panic, they happen in either one of both the following cases: one, when a goroutine is trying to receive data from a channel but there is no other goroutine writing data to the channel; and two, when a goroutine writes to a channel but there is no other goroutine receiving such data.

```golang
func main() {  
    ch := make(chan int)
    ch <- 5
}

func main() {  
    ch := make(chan int)
    <- ch
}
```
Buffered channel deadlock  (fix the code!)

```golang
func main() {
    ch := make(chan int, 5)
    ch <- 1
    ch <- 2
    ch <- 3
    ch <- 4
    ch <- 5
    ch <- 6
    fmt.Println(<-ch)
    fmt.Println(<-ch)
}
```
Check it [here](https://goplay.space/#ITcM9ZXWCy8)

## Iterating a channel with range and close

We've already seen this in another module before so let's recap a bit. In general, a buffered channel can be iterated with the `range` keyword using a `for` loop. Using `range` it is strictly necessary to close the channel using the close channel function with the `close` statement, otherwise `range` will keep trying to get data from the buffered channel (once all elements have been grabbed out from it) which usually results in a deadlock.

`range` can be used with unbuffered channels as well, in this case the loop runs concurrently (with the other goroutines) as long as the channel ramains in a open state. When no more data is pushed into the channel and it has not been properly closed elsewhere (another goroutine) with the `close` function then the application will inevitably fall into a deadlock, given that the `range` will be attempting to receive data from a channel without any data.

Check these examples to put it clear:

```golang
package main

import (
	"fmt"
	"time"
)

func main() {
	ch := make(chan int, 3)
	ch <- 2
	ch <- 2
	ch <- 2
	close(ch)  //Try removing this line
	sum(ch)
}

func sum(ch chan int) {
	sum := 0
	for val := range ch {
		sum += val
	}
	fmt.Printf("Sum: %d\n", sum)
}
```
Check it [here](https://goplay.space/#pSFrY17xQN8)

```golang
package main

import (
	"fmt"
	"time"
)

func main() {
    c := make(chan int)

    go func() {
        for i := 1; i <= 5; i++ {
            c <- i
            time.Sleep(1 * time.Second)
        }
        close(c) //Try removing this line
        fmt.Println("producer finished")
    }()

    for i := range c {
        fmt.Println("i =", i)
    }
    fmt.Println("consumer finished")
}
```
Check it [here](https://goplay.space/#JYGOWG_uCFj)

## Switch select statement

The select statement is used to choose from multiple send/receive channel operations. The select statement blocks until one of the send/receive operations is ready. If multiple operations are ready, one of them is chosen at random. 

This statement looks like a switch but every case will be a channel operation instead. There's also a default case that is really useful to prevent deadlocks, if no other channel operation is ready the default case will remove the block and then continue with the execution.

```golang
package main

import (  
    "fmt"
    "time"
)

func process(ch chan string) {  
    time.Sleep(1050 * time.Millisecond)
    ch <- "process successful"
}

func main() {  
    ch := make(chan string)
    go process(ch)
    for {
        time.Sleep(100 * time.Millisecond)
        select {
        case v := <-ch:
            fmt.Println("received value: ", v)
            return
        default:
            fmt.Println("no value received")
        }
    }
}
```
Check it [here](https://goplay.space/#XQ95WgH3Dcj)

## sync package

Package sync provides basic synchronization primitives such as mutual exclusion locks. Other than the Once and WaitGroup types, most are intended for use by low-level library routines.

The most commonly used features found in the sync package are mutexes and waitgroups but it also includes other primitives that are useful for synchronization tasks in different contexts.

### Mutex

A Mutex is a mutual exclusion lock. By default is initialized in unlocked state. The Mutex type has two methods: Lock, that locks a resource and blocks any goroutine calling the resource until the lock gets removed; and Unlock, that removes the lock set on a shared resource.

The following application panics when mutex is not used to arbitrate the concurrent read and write operations on the map

```golang
package main

import (
	"fmt"
	"sync"
)

func main() {
	m := map[int]int{}

	mux := &sync.Mutex{}

	go writeLoop(m, mux)
	go readLoop(m, mux)

	// stop program from exiting, must be killed
	block := make(chan struct{})
	<-block
}

func writeLoop(m map[int]int, mux *sync.Mutex) {
	for {
		for i := 0; i < 100; i++ {
			mux.Lock()
			m[i] = i
			mux.Unlock()
		}
	}
}

func readLoop(m map[int]int, mux *sync.Mutex) {
	for {
		mux.Lock()
		for k, v := range m {
			fmt.Println(k, "-", v)
		}
		mux.Unlock()
	}
}
```
Check it [here](https://play.golang.org/p/Vj55WK0sUPW)

### WaitGroup

A WaitGroup waits for a collection of goroutines to finish. The main goroutine calls Add to set the number of goroutines to wait for. Then every goroutines runs and calls Done when finished. At the same time, Wait is be used to block the main goroutine until all the other goroutines have finished.

The WaitGroup struct works as a counter of the goroutines the main goroutine has to wait to be finished. With the Add function the counter is incremented, it is the number of goroutines to await for. Each goroutine calls the Done function once its execution finished, then the WaitGroup counter is decremented. With the Wait function the main gorouitne waits until the WaitGroup counter is equal to zero, it means all goroutines have finished execution.

```golang
package main

import (
	"fmt"
	"sync"
)

var wg sync.WaitGroup

func hello(){
   fmt.Println("Goroutine here!")
   wg.Done()
}

func main() {
	wg.Add(3)

	go hello()
	go hello()
	go hello()
	
	wg.Wait()
}
```
Check it [here](https://goplay.space/#fjIrr7LBsjQ)

Check the following example to see goroutines, channels and sync package in action (Run live [here](https://goplay.space/#XiqvW2ZpgL1))

```golang
package main

import (
    "fmt"
    "sync"
    "time"
)

var i int

func work() {
    time.Sleep(250 * time.Millisecond)
    i++
    fmt.Println(i)
}

func routine(command <-chan string, wg *sync.WaitGroup) {
    defer wg.Done()
    var status = "Play"
    for {
        select {
        case cmd := <-command:
            fmt.Println(cmd)
            switch cmd {
            case "Stop":
                return
            case "Pause":
                status = "Pause"
            default:
                status = "Play"
            }
        default:
            if status == "Play" {
                work()
            }
        }
    }
}

func main() {
    var wg sync.WaitGroup
    wg.Add(1)
    command := make(chan string)
    go routine(command, &wg)

    time.Sleep(1 * time.Second)
    command <- "Pause"

    time.Sleep(1 * time.Second)
    command <- "Play"

    time.Sleep(1 * time.Second)
    command <- "Stop"

    wg.Wait()
}
```

Time to have some practice: Fix the following code to see complete output from all goroutines

```golang
package main

import (
	"fmt"
	"math/rand"
	"time"
)

func notify(services ...string) {
	for _, service := range services {
		go func(s string) {
			fmt.Printf("Starting to notifying %s...\n", s)
			time.Sleep(time.Duration(rand.Intn(3)) * time.Second)
			fmt.Printf("Finished notifying %s...\n", s)
		}(service)
	}
	fmt.Println("All services notified!")
}

func main() {
	notify("Service-1", "Service-2", "Service-3")
}
```

### errgroup.Group

errgroup.Group is an alternative to the sync.WaitGroup, it can be used as a mechanism to synchronize goroutines with a few important differences with respect to the WaitGroup. For exmaple, goroutines have to be manually subscribed to the errgroup.Group. Besides, the errgroup.Group not only waits for goroutines to finish but for the first one of them to err, and errgroup.Group also provides a cancelation context to wait for timeouts as well. The errgroup.Group type is part of the golang.org/x/sync/errgroup package

The <b><i>Go</i></b> function is used to subscribe a function as a goroutine in an errgroup.Group.
<br>As in WaitGroup, the <b><i>Wait</i></b> function is used to wait for gorotuines to finish or for the first error to occur in one of them (or a timeout in a errgroup.Group with context)

```golang
package main

import (
	"fmt"
	"net/http"

	"golang.org/x/sync/errgroup"
)

func main() {
	g := new(errgroup.Group)
	var urls = []string{
		"http://www.golang.org/",
		"http://www.google.com/",
		"http://www.somestupidname.com/",
	}
	for _, url := range urls {
		// Launch a goroutine to fetch the URL.
		url := url // https://golang.org/doc/faq#closures_and_goroutines
		g.Go(func() error {
			// Fetch the URL.
			resp, err := http.Get(url)
			if err == nil {
				resp.Body.Close()
			}
			return err
		})
	}
	// Wait for all HTTP fetches to complete.
	if err := g.Wait(); err == nil {
		fmt.Println("Successfully fetched all URLs.")
	}
}
```

## Concurrency patterns

### Async/await

The channel in the main goroutine blocks until the asynchronous function is fisnished

```golang
package main

import (
    "fmt"
    "time"
)

func myAsyncFunction() <-chan int32 {
    r := make(chan int32)
    go func() {
        defer close(r)
        // func() core (meaning, the operation to be completed)
        time.Sleep(time.Duration(rand.Intn(1e3)) * time.Millisecond)
        r <- 2
    }()
    return r
}

func main() {
    r := <-myAsyncFunction()
    fmt.Println(r)
}
```
Check it [here](https://goplay.space/#ceEVRpm--ex)

### Multiple async functions

The main goroutine blocks until all goroutines return their results

```golang
package main

import (
    "fmt"
    "time"
)

func myAsyncFunction(s int32) <-chan int32 {
    r := make(chan int32)
    go func() {
        defer close(r)
        // func() core (meaning, the operation to be completed)
        time.Sleep(time.Duration(rand.Intn(1e3)) * time.Millisecond)
        r <- s
    }()
    return r
}

func main() {
    firstChannel, secondChannel := myAsyncFunction(2), myAsyncFunction(3)
    first, second := <-firstChannel, <-secondChannel    
	fmt.Println(first, second)
}
```
Check it [here](https://goplay.space/#YlXZ6ZtC5vO)

### Multiple async functions race

A select statement is used to gather the result returned by the first goroutine that finishes its job.

```golang
package main

import (
    "fmt"
    "time"
)

func myAsyncFunction(s int32) <-chan int32 {
    r := make(chan int32)
    go func() {
        defer close(r)
        // func() core (meaning, the operation to be completed)
        time.Sleep(time.Duration(rand.Intn(1e3)) * time.Millisecond)
        r <- s
    }()
    return r
}

func main() {
    var r int32
    select {
        case r = <-myAsyncFunction(2):
        case r = <-myAsyncFunction(3):
    }
	
    fmt.Println(r)
}
```
Check it [here](https://goplay.space/#b4WlDbihL_s)

### Timeouts

A time ticker might be useful to fix a timeout for concurrent task to be completed (Try this code with different values for the ticker and the function sleep time)

```golang
package main

import (
	"fmt"
	"time"
	"math/rand"
)

func myAsyncFunction() <-chan bool {
    completed := make(chan bool)
    go func() {
        defer close(completed)
        time.Sleep(time.Duration(rand.Intn(1e3)) * time.Millisecond)
        completed <- true
    }()
    return completed
}

func main() {

	ticker := time.NewTicker(500 * time.Millisecond)
	defer ticker.Stop()
	timeout := make(chan bool) 

	go func() {
		for {
			select {
			case <-myAsyncFunction():
				timeout<-false
				return
			case <-ticker.C:
				timeout<-true
				return
			}
		}
	}()

	if <-timeout{
	   fmt.Println("Timeout!!!")
	}else{
	   fmt.Println("Task was completed")
	}
}
```
Check it [here](https://goplay.space/#eROBdAKrQiB)

### Concurrency pipelines

Informally, a pipeline is a series of stages connected by channels, where each stage is a group of goroutines running the same function. In each stage, the goroutines:

<ul>
<li>receive values from upstream via inbound channels</li>
<li>perform some function on that data, usually producing new values</li>
<li>send values downstream via outbound channels</li>
</ul>

Each stage has any number of inbound and outbound channels, except the first and last stages, which have only outbound or inbound channels, respectively. The first stage is sometimes called the source or producer; the last stage, the sink or consumer.

Generator: function that converts an input source into a channel

```golang
func boring(msg string) <- chan string{
	c := make(chan string)
	go func(){
		for i := 0; ; i++{
			c <- fmt.Sprintf("%s, %d", msg, i)
			time.Sleep(time.Duration(rand.Intn(1e3)) * time.Millisecond)
		}
	}()
	return c
}

func main(){
	c := boring("boring!")
	for i := 0; i < 5; i++{
		fmt.Printf("You say: %q\n", <-c)
	}
	fmt.Println("You're boring; I'm leaving")	
}
```
Check it [here](https://goplay.space/#9oOUuITBbSs)

You can have multiple generators:

```golang
func main(){
	joe := boring("Joe")
	ann := boring("Ann")
	for i:= 0; i < 5; i++{
		fmt.Println(<-joe)
		fmt.Println(<-ann)
	}
	fmt.Println("You're both boring; I'm leaving")
}
```
Check it [here](https://goplay.space/#CLvGaRZRFxq)

Fan-In: A function can read from multiple inputs and proceed until all are closed by multiplexing the input channels onto a single channel that's closed when all the inputs are closed.

```golang
func fanIn(input1, input2 <- chan string) <- chan string{
	c := make(chan string)
	go func(){for{c <- <- input1}}()
	go func(){for{c <- <- input2}}()
	return c
}

func main(){
	c := fanIn(boring("Joe"), boring("Ann"))
	for i:= 0; i < 5; i++{
		fmt.Println(<-c)
	}
	fmt.Println("You're both boring; I'm leaving")
}
```
Check it [here](https://goplay.space/#1WIP50eL4qz)

Example:

```golang
type Message struct{
	str string
	wait chan bool
}

func fanIn(input1, input2 <- chan Message) <- chan Message{
	c := make(chan Message)
	go func(){for{c <- <- input1}}()
	go func(){for{c <- <- input2}}()
	return c
}

func boring(msg string) <- chan Message{
	c := make(chan Message)
	waitForIt := make(chan bool)
	go func(){
		for i := 0; ; i++{
			c <- Message{fmt.Sprintf("%s, %d", msg, i), waitForIt}
			time.Sleep(time.Duration(rand.Intn(1e3)) * time.Millisecond)
			<-waitForIt
		}
	}()
	return c
}

func main(){
	c := fanIn(boring("Joe"), boring("Ann"))
	
	for i := 0; i < 5; i++{
		msg1 := <-c; fmt.Println(msg1.str)
		msg2 := <-c; fmt.Println(msg2.str)
		msg1.wait<-true
		msg2.wait<-true
	}
	
	fmt.Println("You're both boring; I'm leaving")
}
```
Check it [here](https://goplay.space/#CiZ9a_uTQkd)

Fan-Out: Multiple functions can read from the same channel until that channel is closed. This provides a way to distribute work amongst a group of workers to parallelize CPU use and I/O.

```golang
package main

import (
	"fmt"
	"math/rand"
	"time"
)

func generator(hosts ...string) <-chan string {
	out := make(chan string)
	go func() {
		for _, h := range hosts {
			out <- h
		}
		close(out)
	}()
	return out
}

func fanOut(input <-chan string) <-chan string {
	out := make(chan string)
	go func() {
		for n := range input {
			fmt.Printf("Checking if link to host %s is online...\n", n)
			time.Sleep(time.Duration(rand.Intn(1e3)) * time.Millisecond)
			out <- "online!"
		}
		close(out)
	}()
	return out
}

func main() {
	input := generator("google.com", "morioh.com", "blog.golang.org", "goplay.space")
	w1 := fanOut(input)
	w2 := fanOut(input)
	w3 := fanOut(input)
	w4 := fanOut(input)
	fmt.Println("Fnished", <-w1, <-w2, <-w3, <-w4)
}
```
Check it [here](https://goplay.space/#3LhDMVHH9xv)

Example: A complete pipeline with 3 stages, generator, fan-out and fan-in.

```golang
package main

import (
    "fmt"
    "sync"
)

func generator(nums ...int) <-chan int {
    out := make(chan int)
    go func() {
        for _, n := range nums {
            out <- n
        }
        close(out)
    }()
    return out
}

func square(in <-chan int) <-chan int {
    out := make(chan int)
    go func() {
        for n := range in {
            out <- n * n
        }
        close(out)
    }()
    return out
}

func merge(cs ...<-chan int) <-chan int {
    var wg sync.WaitGroup
    out := make(chan int)

    // Start an output goroutine for each input channel in cs.  output
    // copies values from c to out until c is closed, then calls wg.Done.
    output := func(c <-chan int) {
        for n := range c {
            out <- n
        }
        wg.Done()
    }
    wg.Add(len(cs))
    for _, c := range cs {
        go output(c)
    }

    // Start a goroutine to close out once all the output goroutines are
    // done.  This must start after the wg.Add call.
    go func() {
        wg.Wait()
        close(out)
    }()
    return out
}

func main() {
    in := generator(1, 2, 3, 4, 5, 6, 7, 8, 9, 10) //Generator

    //Fan-out
    c1 := square(in)
    c2 := square(in)
    c3 := square(in)
    //Try distributing square work accross some more goroutines

    //Fan-in
    for n := range merge(c1, c2, c3) {
        fmt.Println(n) 
    }
}
```
Check it [here](https://goplay.space/#eYGu4_NSe5h)

### Worker pool (Thread pool)

A worker pool is a collection of goroutines that are supposed to do certain work. Each worker in the pool looks for work to be done in a shared data structure where pending works might be ready to be processed. Once a worker finds work to do, it does it, and once finished it goes back to get more work until all work is already done.

The worker pool performs in a producer-consumer fashion where some generator or producer uses a shared queue to store the work, and then the workers or consumers take work out of the queue. Workers keep consuming work from the queue until no more work is found there.

```golang
package main

import (
    "fmt"
    "time"
)

func worker(id int, jobs <-chan int, results chan<- int) {
    for j := range jobs {
        fmt.Println("worker", id, "started  job", j)
        time.Sleep(time.Second)
        fmt.Println("worker", id, "finished job", j)
        results <- j * 2
    }
}

func main() {

    const numJobs = 5
    jobs := make(chan int, numJobs)
    results := make(chan int, numJobs)

    for w := 1; w <= 3; w++ {
        go worker(w, jobs, results)
    }

    for j := 1; j <= numJobs; j++ {
        jobs <- j
    }
    close(jobs)

    for a := 1; a <= numJobs; a++ {
        <-results
    }
}
```
Check it [here](https://goplay.space/#3rwEG5iIdg7)

Another example, this time using features from sync package

```golang
package main

import (
    "fmt"
    "math/rand"
    "sync"
    "time"
)

type Job struct {
    id       int
    randomNo int
}

type Result struct {
    job       Job
    sumDigits int
}

var jobs = make(chan Job, 10)
var results = make(chan Result, 10)

func digits(num int) int {
    sum := 0
    no := num

    for no != 0 {
        digit := no % 10
        sum += digit
        no /= 10
    }

    time.Sleep(2 * time.Second)
    return sum
}

func worker(wg *sync.WaitGroup) {
    for job := range jobs {
        output := Result{job, digits(job.randomNo)}
        results <- output
    }

    wg.Done()
}

func createWorkerPool(numWorkers int) {
    var wg sync.WaitGroup
    for i := 0; i < numWorkers; i++ {
        wg.Add(1)
        go worker(&wg)
    }
    wg.Wait()
    close(results)
}

func allocateJobs(numJobs int) {
    for i := 0; i < numJobs; i++ {
        randomNum := rand.Intn(999)
        job := Job{i, randomNum}
        jobs <- job
    }
    close(jobs)
}

func result(done chan bool) {
    for res := range results {
        fmt.Printf("Job id %d, input random number %d, sum of digits %d\n", res.job.id, res.job.randomNo, res.sumDigits)
    }
    done <- true
}

func main() {
    starTime := time.Now()
    numJobs := 10
    go allocateJobs(numJobs)
    done := make(chan bool)
    go result(done)
    numWorkers := 10
    createWorkerPool(numWorkers)
    <-done
    endTime := time.Now()
    diff := endTime.Sub(starTime)
    fmt.Println("Total time taken", diff.Seconds(), "seconds")
}
```
Check it [here](https://goplay.space/#vgJA6bWYFW2)

## Useful readings

*  https://slikts.github.io/concurrency-glossary/
* https://www.cs.princeton.edu/courses/archive/fall16/cos418/docs/P1-concurrency.pdf
* https://medium.com/@fsufitch/go-philosophy-share-memory-by-communicating-987f56f0207a
* https://golang.org/doc/codewalk/sharemem/
* https://blog.carlmjohnson.net/post/share-memory-by-communicating/
* https://levelup.gitconnected.com/communicating-sequential-processes-csp-for-go-developer-in-a-nutshell-866795eb879d?gi=cd5dd8140e5c
* http://www.minaandrawos.com/2015/12/06/concurrency-in-golang/
* https://madeddu.xyz/posts/go-async-await/#fn:golang-not-blocking-select
* https://levelup.gitconnected.com/use-go-channels-as-promises-and-async-await-ee62d93078ec
* https://golangbot.com/
* https://medium.com/@fsufitch/go-philosophy-share-memory-by-communicating-987f56f0207a
* https://blog.golang.org/codelab-share
* https://blog.golang.org/concurrency-timeouts
* https://golang.org/pkg/sync
* https://yourbasic.org/golang/wait-for-goroutines-waitgroup/
* https://qvault.io/golang/golang-mutex/
* https://go101.org/article/concurrent-atomic-operation.html
* https://golangdocs.com/atomic-operations-in-golang-atomic-package
* https://blog.golang.org/pipelines
* https://austburn.me/blog/a-better-fan-in-fan-out-example.html
* https://www.cs.cornell.edu/courses/cs3110/2010fa/lectures/lec18.html
* https://syafdia.medium.com/go-concurrency-pattern-worker-pool-a437117025b1
* https://goplay.space
