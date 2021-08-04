# Hello Go!

### A short talk about Go's history
On september 2007, three engineers from Google, Robert Griesemer, Rob Pike and Ken Thompson, shared ideas for a new programming language to address different challenges that engineers in Google were facing in their daily work in relation to multi-core processors, networked systems and large codebases. They wanted a language that could be efficient, scalable and productive at the same time.

<i>"...The goals of the Go project were to eliminate the slowness and clumsiness of software development at Google, and thereby to make the process more productive and scalable. The language was designed by and for people who write and read and debug and maintain large software systems..." -Rob Pike-</i>

Go started being a low profile project that quickly became an ambitious project sponsored by Google. By the end of 2011 Go was oficially released as an open source project and then in March the 28th 2012 the first stable version was released and since then Google publishes a new version update every half a year.

The main goal of creating Go was to combine the best features you can find in other programming languages:
<ul><li>Ease of use together with state-of-the-art productivity</li>
<li>High-level efficiency along with static typing</li>
<li>Advanced performance for networking and the full use of multi-core power</li></ul>

It seems that the main motivation behind Go's development has to do with the dissatisfaction resulting from languages available at the moment, that included features like either efficient compilation, fast execution or easy programming, but not all features at the same time.

Go's sintax is based on C language, but its design is more influenced by languages like Pascal, Modula and Oberon, developed by Niklaus Wirth. The object orientation in Go is similar to that in Smalltalk, except from being able to attach methods to any type. Finally, Go’s concurrency model is mostly taken from Newsqueak, another language developed by Rob Pike and inspired in CSP (Communicating Sequential Processes), by Tony Hoares.

### So what is Go useful for?
Go shines brightest for developing the following application types:
<ul><li>Distributed networked services. Network applications live and die by concurrency, and Go’s native concurrency features — goroutines and channels, mainly— are well suited for such work.</li>
<li>Cloud-native development. Go’s concurrency and networking features, and its high degree of portability, make it well-suited for building cloud-native apps.</li>
<li>Replacements for existing infrastructure. Rewriting aging software in Go provides many advantages: better memory safety, easier cross-platform deployment, and a clean code base to promote future maintenance.</li>
<li>Utilities and stand-alone tools. Go programs compile to binaries with minimal external dependencies. That makes them ideally suited to creating utilities and other tooling, because they launch quickly and can be readily packaged up for redistribution.</li></ul>

### Well, not really good for
<ul><li>Development of device drivers and low level implementations in general.</li>
<li>Embedded systems.</li>
<li>Graphical user interfaces.</li></ul>

### And what about Go features?

Go brings together the ease for programming and dynamic nature of interpreted languages (yes, you're right Python and Javascript) and the security granted by compiled and statically typed languages (C and C++ for instance). Unlike scripting languages, Go code compiles to a fast-running native binary. And unlike C or C++, Go compiles extremely fast enough to make working with Go feel more like working with a scripting language than a compiled language.

Go was designed to run on multiple cores, it is built to support concurrency and scale as cores are added. Go allows for massive multithreading, concurrency, and performance under pressure. Go is meant to be simple to learn, straightforward to work with, and easy to read by other developers. Amazing, isn't it?

Go is particularly ideal for developing cloud-native applications and distributed networked services by allowing developers to bypass writing long lines of code to handle memory leaks or networked applications through native features embedded in the language. Uhmmm I think of garbage collector, built-in concurrency...

Go ensures that your toolbox is compilable across all platforms and on all hardware (Sure Linux and MacOS and yes...Windows). It uses a surprisingly simple package management solution that “just works” and it is extremely portable. Whooaa!!! no need to worry about shared or static libraries.

Go is a Swiss Army knife, but one that has stripped off all of the overhead and extra junk that has accreted onto programming platforms over the past few decades. For people who already know the basics of programming or a few other languages, learning Go takes a few hours at most. Once you know its tricks, you’re ready to code. I'm not saying it does not take while till you master it, it's a process just like everything in life...

Go apps will work faster (not faster than C or C++...) and won’t require any interpreter or virtual machine (yes Java and C# I'm talking about you!), it actually compiles to native machine code and the compiler has all the required tools to have our executables ready for any OS and hardware platform.

Go's main virtues are its simplicity and multi-functionality, aspects pursued by its creators from the beginning.

### Advantages of Go over other programming languages:
<ul>
<li>Easy to Use and Read: Go’s syntax is simple, with a forgiving learning curve that makes it more accessible to novice programmers. It also helps that there aren’t too many complex functions to learn (Only 25 keywords).</li>
<li>Impressive Standard Library: Go users have access to an impressive standard library that comes packaged with the language, which saves the trouble of importing or learning complex secondary libraries. Go’s standard library is sophisticated but not confusing, helping reduce the risk of issues from conflicting function names.</li>
<li>Strong Security: Thanks to simpler code, statically typed and garbage collector features.</li>
<li>Support by Google and an ever growing community.</li>
<li>Intuitive documentation: Go has standard policies for documenting all included functions and libraries.</li> 
<li>While Go was originally for systems programming, it's been adopted for microservices. This is due to its elegant concurrency model, simple binary deployments, low number of external dependencies, fast performance, and runtime efficiency.</li>
<li>Go is perfect for running an app in the background as a single process. This is possible thanks to goroutines, which are used instead of threads and require much less RAM due to their non-system thread nature. This is why the risk of a Go app crashing due to lack of memory is lower.</li>
<li>Built-in dependency management</li>
</ul>

### Features for developers:
<ul>
<li>Automatic documentation</li> 
<li>Static code analysis</li>
<li>Embedded testing and benchmarking environment</li>
<li>Race condition detection</li>
<li>A lightweight type system expressive enough to classify and differentiate objects (variables, functions, etc.)</li>
<li>Native concurrency that allows for faster and background execution of software routines</li>
<li>Garbage collector for rational use of memory</li>
<li>Multiplatform, that allows to use aplications in nearly any available system</li>
<li>Good performance, in terms of speed compared to other programming languages</li>
<li>An extensive standard library (specially for networking)</li>
<li>Pointers (don't hate me I'm a former C++ dev..)</li>
</ul>


### Go is not perfect...
<ul>
<li>Lack of a native GUI framework</li>
<li>Automatic hidding of unused imported files</li>
<li>Manual error handling</li>
<li>Interfaces are implicit. Sometimes it's hard to tell if a struct type implements an interface.</li>
<li>Go doesn't support overloading.</li>
<li>Too Simple:  As easy as it is to pick up Go, its simplicity also means it’s not as versatile as other languages. (less abstractions to do more with less)</li>
<li>Go does not include generics, and the stewards of the language are against adding them, on the basis that generics would compromise the language’s simplicity.</li>
<li>Go binaries are statically compiled by default, meaning that everything needed at runtime is included in the binary image. This approach simplifies the build and deployment process, but at the cost of a simple “Hello, world!” weighing in at around 1.5MB on 64-bit Windows.</li>
<li>Although Go can talk to native system functions, it was not designed for creating low-level system components, such as kernels or device drivers, or embedded systems.</li>
<li>Pre-declared (built-in) identifiers can be shadowed (var true = 0!!!)</li>
</ul>

## Installing Go in your system

If you love reading docs from the source: ([Getting started](https://golang.org/doc/tutorial/getting-started))

### Windows

It is strongly recommended to use chocolatey package manager: <b>choco install golang</b>
If you use Goland, you can install Go directly using this IDE.

### IDE's for Go

<b>[VSCode](https://code.visualstudio.com/docs/languages/go)</b>
<ol>
<li>Ctrl+Shift+P -> Go: Install/Update Tools -> Check all boxes and Install</li>
<li>Install extensions for Go programming language</li>
</ol>

<b>[Goland](https://www.jetbrains.com/help/go/quick-start-guide-goland.html)</b>

You need to specify the location of the Go SDK. You can specify a local path to the SDK or download it. To set the Go SDK, open settings Ctrl+Alt+S and navigate to Go | GOROOT. 

Click the Add SDK button and select between two options:

Local: use a local SDK copy. In the file browser, navigate to the SDK version that is on your hard drive.
Download: download the SDK. In the Location field, specify the path for the SDK. To use a file browser, click the Browse icon. Click OK.

## Go modules

Modules is Go's new dependency management system that makes dependency version information explicit and easier to manage. Modules replaced all style dependency management using GOPATH and dep.

A module is a collection of packages that are released, versioned and distributed together. Modules may be downloaded directly from version control repositories or from module proxy servers. In that way, a module is a collection of Go packages stored in a file tree with a <b>go.mod</b> file at its root. The go.mod file defines the module’s module path, which is also the import path used for the root directory, and its dependency requirements, which are the other modules needed for a successful build. Each dependency requirement is written as a module path and a specific semantic version (MAJOR, MINOR, PATCH, etc.).

go.mod file is a  UTF-8 encoded text file that defines a module and is located in the root directory of the module. The go.mod file is line-oriented, each line holds a single directive, made up of a keyword followed by arguments.

(go.mod file image)

Each package within a module is a collection of source code files in the same directory that are compiled together. A package path is the module path (module path is the canonical name for a module, declared with the module directive in the module's go.mod file) joined with the subdirectory containing the package (relative to the module root). For example, the module <i>golang.org/x/net</i> contains a package in the directory <i>html</i>. That package's path is <i>golang.org/x/net/html</i>.

### Creating a module in Go

<b>go mod init <module_name></b>

This command creates the module and its go.mod file. <module_name> is an optional parameter, if ommited init will attempt to infer the module path using import comments in .go files, vendoring tool configuration files, and the current directory (if in <i>GOPATH</i>). Then, it is recommended not to ommit the module name in an empty project.

### Useful commands working with go modules

<b>go mod tidy</b>

This command is mainly used to download all module's dependencies and to update the go.mod file each time a new dependency is added to the module, but it is also useful for:

<ol>
<li>Making sure that the file go.md reflects all possible combinations of OS, architecture and build tags.</li>
<li>go mod tidy creates the file go.sum that will manage all versions and their integrity using hash calculations. go.sum file allows to add new dependencies with the command <i>go get...</i></li>
<li>Removes unused dependencies</li>
</ol>

<b>go mod vendor</b>

The go mod vendor command constructs a directory named vendor in the main module's root directory that contains copies of all packages needed to support builds and tests of packages in the main module. Packages that are only imported by tests of packages outside the main module are not included.

When vendoring is enabled, the go command will load packages from the vendor directory instead of downloading modules from their sources into the module cache and using packages those downloaded copies.

go mod vendor also creates the file <i>vendor/modules.txt</i> that contains a list of vendored packages and the module versions they were copied from.

<b>go list -m all</b>

Prints the current module's dependencies.

## Basic structure of a .go file

<ul>
<li>package: Indicates the package to which the current .go source file belongs (compilation unit)</li>
<li>import list: Lists the dependencies necessary for the current .go source file</li>
<li>Logic: variables, constants, functions, type definitions, etc.</li>
</ul>

The main.go file must include a main function

## Example: Hello World

```golang
package main

import(
	"fmt"
	"time"
)

func main(){
	fmt.Println("Hello Go!")
	fmt.Printf("It is %d:%d", time.Now().Hour(), time.Now().Minute())
}
```
Try the code online [here](https://goplay.space/#Aq19DbZuRIS)

## Build and compilation

<b>Compilation of a single Go package: Generates object files</b>

```bash
go tool compile [flags] files...
```

Object files can be combined into a package archive or passed directly to the linker

<ol>
<li>go tool compile -pack files... (Write a package (archive) file rather than an object file)</li>
<li>go tool link -o executable_file_name main.a (Reads the Go archive or object for a package main, along with its dependencies, and combines them into an executable binary)</li>
</ol>

<b>Build and install</b>

-go build command compiles the packages, along with their dependencies, but it doesn't install the results.
<br>-go install command compiles and installs the packages.

```bash
go build
```

Install:
<ol>
<li>Discover the install path: go list -f '{{.Target}}'</li>
<li>Add the Go install directory to your system's shell path: set PATH=%PATH%;C:\path\to\your\install\directory
   <br>If you already have a bin directory in your system: go env -w GOBIN=C:\path\to\your\bin</li>
</ol>

```bash
go install
```

<b>Run your main.go</b>

```bash
go run main.go 
```

## Useful readings

* https://yalantis.com/blog/why-use-go/
* https://stackoverflow.blog/2020/11/02/go-golang-learn-fast-programming-languages/
* https://www.ionos.es/digitalguide/servidores/know-how/golang/
* https://go.dev/solutions/google/
* https://www.infoworld.com/article/3198928/whats-the-go-language-really-good-for.html
* https://www.simplilearn.com/go-programming-language-article
* https://qarea.com/blog/the-evolution-of-go-a-history-of-success
* https://golang.design/history/
* https://blog.learngoprogramming.com/about-go-language-an-overview-f0bee143597c
* https://devopedia.org/go-language
* https://github.com/ksimka/go-is-not-good
* https://blog.golang.org/using-go-modules
* https://golang.org/ref/mod
* https://blog.friendsofgo.tech/posts/go-modules-en-tres-pasos/
* https://go101.org/article/blocks-and-scopes.html
* https://golang.org/cmd/compile/
* https://golang.org/cmd/link/
* https://golang.org/doc/tutorial/compile-install
* https://golang.org/pkg/go/build/