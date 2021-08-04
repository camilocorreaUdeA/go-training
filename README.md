# Go Training

# Welcome to this Go training!

<p>Greetings! If you are here it means you want to learn a fascinating language called Go and start building amazing stuff with it.
This taining is a first entry point to get you immersed in Go, we will provide you with concise and precise information about the language so you would hopefully not need to look for additional information out there. Our goal is to have you ready to start practicing with some exercises and laboratories that we have already prepared so you can challenge yourself and why not move your career in a new direction.

## What is this all about?

<p>This is definitely not a crash-course and not either a pre-certification alike course. It is just us sharing with you the path we followed to learn Go. We are not experts but we are avid enthusiasts that enjoy growing our skills and that have found in Go a really powerful tool to build the technology that will (already started!) revolutionize the world. Here are a few reasons to consider getting skilled in Go:

<p>In the latest years Go has been gaining attraction and its audience is becoming bigger day by day and has been rapidly reaching the top in worldwide developers' choices lists, that's remarkable news having in mind that Go is a pretty recent language, with only a decade and a bit since it saw the light. Part of this success is its maturation at an incredibly fast pace, due in large part to its thorough and comprehensive design process from the very start (a language thought for large distributed systems!). Therefore it is not surprising that companies like Google, Uber, Dropbox, Twitch, Soundcloud, Dailymotion, MercadoLibre, The New York Times and many more use Go as their domain language or at least for the development of part of their systems.

<p>Go is a language that gets along pretty well with almost every technology out there and the ever growing Go community is always in the front line to engage anything outside the language domain. Then, the learning curve for adopting any technology with Go is not considerable since it is almost completely sure that there's a package or library ready to be picked up to ease your life.

<p>Last but not least, our clients in Endava are continously asking for Go and as the time passes there will be more requirements in this language, so in this prespective we need to be ready and by the way position our DU as a development hub for Go projects. By the nature of clients that use Go (fintechs, media streamers, high), and most important the size and importance of such clients ;-), having a consolidated teams of well-versed individuals in Go increases the possiblities to keep our hands dirty in projects that astonish the tech audience (really!!!)

<p>Once having sold out the reasons for Go let's get into the training. The present training has been divided into 3 sections, in the very beggining there is a basic approach to the language where we interact with the syntax, the language primitives in general and most important we get immersed in Go's philosophy. For the second part the aim is to adopt the features that make Go unique and different to other languages, particularly, object oriented languages so the most important thing here is learning to make things the Go way, use interfaces and start diving in its concurrency model that is one of the greatest features included in the language. For the final part we considered interesting to let you <b><i>"Go"</i></b> by yourselves and let you explore and interact with the tools we have at hand to start building real world things with Go, in this part we expect you to take your own decisions to solve a short challenge.

<p>To make it more formal let's have a schedule for the training:

## Week One: A tour of Go

### A little bit of Go history 

First, get to know Go.  Let's check a brief review about Go's history and a quick checklist of its most prominent features. [Nice to meet you Go!](https://bitbucket.endava.com/projects/MED/repos/med-golang-bootcamp/browse/A_tour_of_Go/Hello_Go.md)

### Set up your Go environment

[Here](https://bitbucket.endava.com/projects/MED/repos/med-golang-bootcamp/browse/A_tour_of_Go/Hello_Go.md) you will find the steps to put your environment ready to Go. But you can always check the official documents and some other insightful videos:
* [Download and Install](https://golang.org/doc/install)
* [Set up your local env](https://www.digitalocean.com/community/tutorials/how-to-install-go-and-set-up-a-local-programming-environment-on-windows-10-es)
* [Introduction and Environment Setup (Windows & Mac)](https://www.youtube.com/watch?v=dgIh-VYcWYw)
* [Go in VSCode](https://www.youtube.com/watch?v=TfCMweSHWHw)
* [Go in Goland](https://www.youtube.com/watch?v=AufkDPEI2qA)

### Start working with Go

Hmmm I guess everything is ready and fine, Can I start coding in Go? Yeah! sure, but first learn how to start a Go project with Go modules, then create your first <i>.go</i> file and add packages to your application. And also check some compiler utilities that will help you in your way. Yes you're right we also have it [here](https://github.com/camilocorreaUdeA/go-training/blob/main/A_tour_of_Go/Hello_Go.md) for you! Let's run your own Go Hello World!!!

We're not selfish at all, you might want to check pretty useful information by yourself in these resources:
* [How Do You Structure Your Go Apps?](https://www.youtube.com/watch?v=1rxDzs0zgcE)
* [Your First Go File](https://www.youtube.com/watch?v=RI9ngRqn9N4)
* [EVERYTHING You SHOULD know about Go Modules](https://www.youtube.com/watch?v=Z1VhG7cf83M)

### Fresh start with the basics

Everything you need to start writing code in Go: Variables, constants, primitive data types, operators, functions, flow control statements, pointers and built-in data structures. Let's check our [compilation](https://bitbucket.endava.com/projects/MED/repos/med-golang-bootcamp/browse/A_tour_of_Go/Basic_types_and_Variables.md) and practice with the lab [exercises](https://bitbucket.endava.com/projects/MED/repos/med-golang-bootcamp/browse/A_tour_of_Go/labs/basic_types_and_variables.md)

In this part you will learn go syntax, and the very basic features that Go offers to start building applications. Please put special attention to the built-in data structuress: arrays, slices and maps. And make sure to have unserstood all the magic behind Go structs and pointers.

Ok, take me [there!](https://bitbucket.endava.com/projects/MED/repos/med-golang-bootcamp/browse/A_tour_of_Go/FS_and_DS.md)

Practice: [Here](https://bitbucket.endava.com/projects/MED/repos/med-golang-bootcamp/browse/A_tour_of_Go/labs/fs_and_ds.md) we have a few tasks for you. Go for them and have fun!

## Week Two: Unique features in Go 

### Methods and Interfaces

<p>Go is not an OOP language, or at least its OOP is unlike what we've seen in Java, Python or C++. In fact, we strongly recommend to get rid-off of some practices and patterns that you're used to implement with other languages because even if they're feasible in Go it does not always mean that you're leveraging Go features correctly or perhaps not coding in <b><i>idiomatic Go</i></b>.

Besides, <p>Go is about <b>Composition</b>, though Go does not have classes you can always define your own types using Go structs, receivers and methods. You can also take advantage of already defined functionalities by composing structs. And, last but not least, define common types via implementation of interfaces.

Let's check what we have compiled for you: [Methods and Interfaces](https://bitbucket.endava.com/projects/MED/repos/med-golang-bootcamp/browse/Unique_Go_features/Methods_and_Interfaces.md) and have fun solving the lab practice [here](https://bitbucket.endava.com/projects/MED/repos/med-golang-bootcamp/browse/Unique_Go_features/labs/methods_and_interfaces.md)

Pretty good stuff available for everyone:
* [Idiomatic Go Tricks](https://www.youtube.com/watch?v=yeetIgNeIkc)
* [Adopte la Interfaz](https://www.youtube.com/watch?v=xyDkyFjzFVc&pp=ugMICgJlcxABGAE%3D)
* [Understanding Interfaces in Golang](https://www.youtube.com/watch?v=gfoVLXQ5ujM)
* [Interfaces](https://www.youtube.com/watch?v=lh_Uv2imp14)
* [Go by Example: Interfaces](https://gobyexample.com/interfaces)

### Error handling in Go

Bad news guys, Go doesn't have <i>try, catch, throw</i>, or similar statements for error handling. Yes we know you are used to these sticky words but the good news is that in Go you can handle errors in your own way, and we are sure that you love to have a fine-grained management of the errors encountered in your applications. [Here](https://bitbucket.endava.com/projects/MED/repos/med-golang-bootcamp/browse/Unique_Go_features/Error_Handler.md) we abbreviate for you the error handling process in Go, it is a basic starting point but it is all you will need to specialize your own error handling techniques.

We are sure you die to challenge yourself, so let's solve the lab ----> [here](https://bitbucket.endava.com/projects/MED/repos/med-golang-bootcamp/browse/Unique_Go_features/labs/error_handler.md)

You better check the following to have a better unsertanding and sooner than later master the error handling in Go:
* [Error handling and Go](https://blog.golang.org/error-handling-and-go)
* [Working with Errors in Go 1.13](https://blog.golang.org/go1.13-errors)
* [Error handling in Golang](https://gabrieltanner.org/blog/golang-error-handling-definitive-guide)
* [Why Go's Error Handling is Awesome](https://rauljordan.com/2020/07/06/why-go-error-handling-is-awesome.html)
* [Error handling in Golang](https://blog.logrocket.com/error-handling-in-golang/)

### Concurrency

Go is about <b>Concurrency</b>. As we have mentioned several times, Go was designed for high-duty time-critical distibuted systems, it means that Go is for applications that are expected to respond to a myriad of requests without hanging-up or crashing. Fortunately, one of the most amazing features in Go (if not the very most) is the built-in support for concurrency, for this mmatter Go provides  with light-weight threads called Goroutines and channels that used in conjuction definitely make programming a concurrent application a game for your children. It is worth mentioning that concurrency in Go leverages the philosophy of sharing by communicating so hopefully if you do things well you will not face neither data races nor run into deadlocks in your apps. Let's check this [short lesson](https://bitbucket.endava.com/projects/MED/repos/med-golang-bootcamp/browse/Unique_Go_features/Concurrency.md) and later start practicing to master your concurrency skills in Go ([Pratice lab](https://bitbucket.endava.com/projects/MED/repos/med-golang-bootcamp/browse/Unique_Go_features/labs/concurrency.md)). As always there are interesting things you can check by yourself, here´s our selection this time:

* [Concurrency Made Easy (Practical Tips For Effective Concurrency In Go)](https://www.youtube.com/watch?v=DqHb5KBe7qI)
* [Concurrency Patterns In Go](https://www.youtube.com/watch?v=YEKjSzIwAdA)
* [Google I/O 2012 - Go Concurrency Patterns](https://www.youtube.com/watch?v=f6kdp27TYZs&t=1180s)
* [Concurrency is not Parallelism by Rob Pike](https://www.youtube.com/watch?v=oV9rvDllKEg)
* [Google I/O 2013 - Advanced Go Concurrency Patterns](https://www.youtube.com/watch?v=QDDwwePbDtw&t=165s)
* [Concurrency in Golang: A Simple, Practical Example](https://www.youtube.com/watch?v=3atNYmqXyV4)

### Go's standard library

Go has a bunch of useful functionalities ready to be integrated in your applications, you just need to import the required package and start crafting your apps. The tools in the standard library are meant to boost ypur productivity and ease the development process, they are an extension of the language that includes core functionalities for common tasks like string and slice manipulation, events logging, mathematical calculations, time measuring, input/output interfacing and formatting, interfaces to OS functions, file handling, and other not so common but amazingly useful for taks like encoding, cryptography, networking, file compressing, etc.

At this step in the learning path it is up to you to check what's in the standard library, we made a comprehensive list with the most commonly used features and provide with examples to show you how to use them. Check it [here!](https://bitbucket.endava.com/projects/MED/repos/med-golang-bootcamp/browse/Unique_Go_features/Standard_lib.md). But if you want to go to the source, do not hesitate to visit the official documentation: [Go standard library](https://pkg.go.dev/std).

Things are getting pretty serious know but we all are confident that you are learning really fast, we've prepared [these](https://bitbucket.endava.com/projects/MED/repos/med-golang-bootcamp/browse/Unique_Go_features/labs/std_lib.md) challenges for you to show us you master Go's std lib.

## Week Three: Web development with Go

Congratulations! You've reached the top. Now it is time to put all you've learned in a real-world challenge. At this stage we are convinced that you have been giving the best from you in the past weeks but there's one more step to show waht you're made of and that you're indeed ready to Go. In this part of the training we just provide you with a list of awesome packages used in development of web applications with Go. The [list](https://bitbucket.endava.com/projects/MED/repos/med-golang-bootcamp/browse/Web_develoopment_Go/Backend.md) includes third-party packages featuring webframeworks, database drivers, microservices, serverless and cloud frameworks, message brokers, queues, caches, etc. And from our part, the last thing we do is presenting to you a small challenge and it's up to you to choose the tools that work the best for the implementation of a solution.

The last [task](https://bitbucket.endava.com/projects/MED/repos/med-golang-bootcamp/browse/Web_Development_Go/labs/backend.md) but not the least. Have fun and show us what you're made of!.

<br>That's all for now, thanks for joining us in this training, we hope you enjoyed your time and learned a lot!
