# Webframeworks

The following packages written in Go are really useful for esasing the development of REST APIs. Among many different features the often include handler registering, routes mapping and matching, serving static files, adding middleware, routes multiplexing, parsing headers and queries, etc.

There are a bunch of webframeworks written in Go available out there and ready to be used, choosing one among the others strongly depends on the developer's preferences, the feautres offered by the framework and the nature of the API itself.

The following list is a reference to well-known Go webframeworks very popular and widely used by web developers that choose Go as the language for their backends.

## gorilla/mux

https://www.gorillatoolkit.org/pkg/mux
<br>https://github.com/gorilla/mux

Package gorilla/mux implements a request router and dispatcher for matching incoming requests to their respective handler.

The name mux stands for "HTTP request multiplexer". Like the standard http.ServeMux, mux.Router matches incoming requests against a list of registered routes and calls a handler for the route that matches the URL or other conditions. The main features are:

<ul>
<li>It implements the http.Handler interface so it is compatible with the standard http.ServeMux.</li>
<li>Requests can be matched based on URL host, path, path prefix, schemes, header and query values, HTTP methods or using custom matchers.</li>
<li>URL hosts, paths and query values can have variables with an optional regular expression.</li>
<li>Registered URLs can be built, or "reversed", which helps maintaining references to resources.</li>
<li>Routes can be used as subrouters: nested routes are only tested if the parent route matches. This is useful to define groups of routes that share common conditions like a host, a path prefix or other repeated attributes. As a bonus, this optimizes request matching.</li>
</ul>

Get the package: <b>go get -u github.com/gorilla/mux</b>

Tutorials, guides, opinions, tips and interesting stuff in general:

https://semaphoreci.com/community/tutorials/building-and-testing-a-rest-api-in-go-with-gorilla-mux-and-postgresql
<br>https://gowebexamples.com/routes-using-gorilla-mux/
<br>https://subscription.packtpub.com/book/web_development/9781787286740/1/ch01lvl1sec19/implementing-http-request-routing-using-gorilla-mux
<br>https://opensource.com/article/18/8/http-request-routing-validation-gorillamux
<br>https://shouts.dev/routing-in-go-using-gorilla-mux

```golang
package main

import (
	"net/http"
	
	"github.com/gorilla/mux"
)

func main() {
    r := mux.NewRouter()
    r.HandleFunc("/", HomeHandler)
    r.HandleFunc("/products", ProductsHandler)
    r.HandleFunc("/articles", ArticlesHandler)
    http.Handle("/", r)
}
```

## Echo: High performance, extensible, minimalist Go web framework

https://echo.labstack.com/
<br>https://github.com/labstack/echo

Feature overview:

<ul>
<li>Optimized HTTP router which smartly prioritize routes</li>
<li>Build robust and scalable RESTful APIs</li>
<li>Group APIs</li>
<li>Extensible middleware framework</li>
<li>Define middleware at root, group or route level</li>
<li>Data binding for JSON, XML and form payload</li>
<li>Handy functions to send variety of HTTP responses</li>
<li>Centralized HTTP error handling</li>
<li>Template rendering with any template engine</li>
<li>Define your format for the logger</li>
<li>Highly customizable</li>
<li>Automatic TLS via Let’s Encrypt</li>
<li>HTTP/2 support</li>
</ul>

Get the package: <b>go get github.com/labstack/echo/{version}</b>

Tutorials, guides, opinions, tips and interesting stuff in general:

https://betterprogramming.pub/intro-77f65f73f6d3
<br>https://www.netlify.com/blog/2016/10/20/building-a-restful-api-in-go/
<br>https://echo.labstack.com/cookbook/twitter/
<br>https://www.restapiexample.com/golang-tutorial/consume-restful-apis-using-echo-golang/
<br>https://jonathanmh.com/building-a-golang-api-with-echo-and-mysql/

```golang
package main

import (
  "net/http"
  "github.com/labstack/echo/v4"
  "github.com/labstack/echo/v4/middleware"
)

func main() {
  // Echo instance
  e := echo.New()

  // Middleware
  e.Use(middleware.Logger())
  e.Use(middleware.Recover())

  // Routes
  e.GET("/", hello)

  // Start server
  e.Logger.Fatal(e.Start(":1323"))
}

// Handler
func hello(c echo.Context) error {
  return c.String(http.StatusOK, "Hello, World!")
}
```

## Fiber: An Express-inspired web framework written in Go

https://gofiber.io/
<br>https://github.com/gofiber/fiber

Fiber is a Go web framework built on top of Fasthttp, the fastest HTTP engine for Go. It's designed to ease things up for fast development with zero memory allocation and performance in mind.

Features:

<ul>
<li>Robust routing</li>
<li>Serve static files</li>
<li>Extreme performance</li>
<li>Low memory footprint</li>
<li>API endpoints</li>
<li>Middleware & Next support</li>
<li>Rapid server-side programming</li>
<li>Template engines</li>
<li>WebSocket support</li>
<li>Rate Limiter</li>
<li>Translated in 15 languages</li>
</ul>

Get the package: <b>go get -u github.com/gofiber/fiber/v2</b>

Tutorials, guides, opinions, tips and interesting stuff in general:

https://enbonnet.me/article/53/construir-api-golang-con-fiber
<br>https://blog.logrocket.com/express-style-api-go-fiber/
<br>https://tutorialedge.net/golang/basic-rest-api-go-fiber/
<br>https://dev.to/koddr/build-a-restful-api-on-go-fiber-postgresql-jwt-and-swagger-docs-in-isolated-docker-containers-475j
<br>https://dev.to/devsmranjan/golang-build-your-first-rest-api-with-fiber-24eh

```golang
package main

import (
    "log"

    "github.com/gofiber/fiber/v2"
)

func main() {
    app := fiber.New()

    app.Get("/", func (c *fiber.Ctx) error {
        return c.SendString("Hello, World!")
    })

    log.Fatal(app.Listen(":3000"))
}
```

## Gin: Gin is a web framework written in Golang. It features a Martini-like API, but with performance up to 40 times faster than Martini.

https://gin-gonic.com/
<br>https://github.com/gin-gonic/gin

Features:

<ul>
<li>Fast</li>
<li>Middleware support</li>
<li>Crash-free</li>
<li>JSON validation</li>
<li>Routes grouping</li>
<li>Error management</li>
<li>Rendering built-in</li>
<li>Extendable</li>
</ul>

Get the package: <b>go get -u github.com/gin-gonic/gin</b>

Tutorials, guides, opinions, tips and interesting stuff in general:

https://blog.logrocket.com/how-to-build-a-rest-api-with-golang-using-gin-and-gorm/
<br>https://dev.to/techschoolguru/implement-restful-http-api-in-go-using-gin-4ap1
<br>https://dev.to/faruq2/how-to-build-a-crud-rest-api-with-go-gin-and-fauna-37o6
<br>https://levelup.gitconnected.com/build-your-first-rest-api-in-go-language-using-gin-framework-827aadc14e07
<br>https://semaphoreci.com/community/tutorials/building-go-web-applications-and-microservices-using-gin
<br>https://www.agiratech.com/blog/building-restful-api-service-golang-using-gin-gonic/

```golang
package main

import "github.com/gin-gonic/gin"

func main() {
	r := gin.Default()
	r.GET("/ping", func(c *gin.Context) {
		c.JSON(200, gin.H{
			"message": "pong",
		})
	})
	r.Run() // listen and serve on 0.0.0.0:8080 (for windows "localhost:8080")
}
```

## Beego: An open source framework to build and develop your applications in the Go way

https://beego.me/
<br>https://github.com/beego/beego

Beego is used for rapid development of enterprise application in Go, including RESTful APIs, web apps and backend services.
It is inspired by Tornado, Sinatra and Flask. beego has some Go-specific features such as interfaces and struct embedding.

Features:

<ul>
<li>RESTful support</li>
<li>MVC architecture</li>
<li>Modularity</li>
<li>Auto API documents</li>
<li>Annotation router</li>
<li>Namespace</li>
<li>Powerful development tools</li>
<li>Full stack for Web & API</li>
</ul>

Get the package: <b>go get github.com/beego/beego/v2@v2.0.0</b>

Tutorials, guides, opinions, tips and interesting stuff in general:

https://www.programmersought.com/article/38684462463/
<br>https://github.com/fmorenovr/restfulapi-BeeGo
<br>https://medium.com/mobile-development-group/how-beego-swagger-can-make-coding-an-api-easier-227b56055cbc

```golang
package main

import (
    "github.com/beego/beego/v2/server/web"
)

type MainController struct {
    web.Controller
}

func (this *MainController) Get() {
    this.Ctx.WriteString("hello world")
}

func main() {
    web.Router("/", &MainController{})
    web.Run()
}
```

# Databases

So far you have noticed it is not our goal to teach you how to use Go for backend, but we will provide you with our recommendations in a curated list with the packages and frameworks that will make your life easier. No worries! Here's also a list of Go database drivers for both SQL and NoSQL top-notch db engines and systems.

# SQL

Although Go's standard library comes with a package that supports SQL database management. The sql package must be used in conjunction with a database driver, then support for the different flavors of SQL-like databases relies on third-party packages that implement those drivers.

## database/sql

https://golang.org/pkg/database/sql/

We strongly believe you might check that link to form yourself an idea of what Go has for you to deal with db duties.

## go-sql-driver/mysql

https://github.com/go-sql-driver/mysql

Features:

<ul>
<li>Lightweight and fast</li>
<li>Native Go implementation. No C-bindings, just pure Go</li>
<li>Connections over TCP/IPv4, TCP/IPv6, Unix domain sockets or custom protocols</li>
<li>Automatic handling of broken connections</li>
<li>Automatic Connection Pooling (by database/sql package)</li>
<li>Supports queries larger than 16MB</li>
<li>Full sql.RawBytes support.</li>
<li>Intelligent LONG DATA handling in prepared statements</li>
<li>Secure LOAD DATA LOCAL INFILE support with file allowlisting and io.Reader support</li>
<li>Optional time.Time parsing</li>
<li>Optional placeholder interpolation</li>
</ul>

Get the package: <b>go get -u github.com/go-sql-driver/mysql</b>

Tutorials, guides, opinions, tips and interesting stuff in general:

https://tutorialedge.net/golang/golang-mysql-tutorial/
<br>https://www.golangprograms.com/example-of-golang-crud-using-mysql-from-scratch.html
<br>https://golangbot.com/connect-create-db-mysql/
<br>http://learningprogramming.net/category/golang/golang-and-mysql/

```golang
import (
	"database/sql"
	"time"

	_ "github.com/go-sql-driver/mysql"
)

// ...

db, err := sql.Open("mysql", "user:password@/dbname")
if err != nil {
	panic(err)
}
// ...
db.SetConnMaxLifetime(time.Minute * 3)
db.SetMaxOpenConns(10)
db.SetMaxIdleConns(10)
```

## go-mssqldb: A pure Go MSSQL driver for Go's database/sql package

https://github.com/denisenkom/go-mssqldb

Features:

<ul>
<li>Can be used with SQL Server 2005 or newer</li>
<li>Can be used with Microsoft Azure SQL Database</li>
<li>Can be used on all go supported platforms (e.g. Linux, Mac OS X and Windows)</li>
<li>Supports new date/time types: date, time, datetime2, datetimeoffset</li>
<li>Supports string parameters longer than 8000 characters</li>
<li>Supports encryption using SSL/TLS</li>
<li>Supports SQL Server and Windows Authentication</li>
<li>Supports Single-Sign-On on Windows</li>
<li>Supports connections to AlwaysOn Availability Group listeners, including re-direction to read-only replicas.</li>
<li>Supports query notifications</li>
</ul>

Get the package: <b>go get github.com/denisenkom/go-mssqldb</b>

Tutorials, guides, opinions, tips and interesting stuff in general:

https://mathaywardhill.com/2017/04/27/get-started-with-golang-and-sql-server-in-visual-studio-code/
<br>http://jenicaandpatrick.com/how-to-query-a-ms-sql-server-database-in-go-golang/
<br>https://bsilverstrim.blogspot.com/2015/08/golang-and-mssql-databases-example.html

```golang
query := url.Values{}
query.Add("app name", "MyAppName")

u := &url.URL{
  Scheme:   "sqlserver",
  User:     url.UserPassword(username, password),
  Host:     fmt.Sprintf("%s:%d", hostname, port),
  // Path:  instance, // if connecting to an instance instead of a port
  RawQuery: query.Encode(),
}
db, err := sql.Open("sqlserver", u.String())
```

## go-sqlite3: A cgo package for SQLite databases in Go. Needs gcc to be compiled.

https://github.com/mattn/go-sqlite3

Get the package: <b>go get github.com/mattn/go-sqlite3</b>

Tutorials, guides, opinions, tips and interesting stuff in general:

https://astaxie.gitbooks.io/build-web-application-with-golang/content/en/05.3.html
<br>https://medium.com/@orlmonteverde/api-rest-con-go-golang-y-sqlite3-e378af30719c
<br>https://www.codeproject.com/Articles/5261771/Golang-SQLite-Simple-Example
<br>https://www.bogotobogo.com/GoLang/GoLang_SQLite.php
<br>https://www.ardanlabs.com/blog/2020/11/working-with-sqlite-using-go-python.html
<br>https://www.thepolyglotdeveloper.com/2017/04/using-sqlite-database-golang-application/

```golang
import (
	"database/sql"
	"fmt"
	_ "github.com/mattn/go-sqlite3"
	"log"
	"os"
)

// ...
db, err := sql.Open("sqlite3", "./foo.db")
if err != nil {
	log.Fatal(err)
}
defer db.Close()

sqlStmt := `
create table foo (id integer not null primary key, name text);
delete from foo;
`
_, err = db.Exec(sqlStmt)
if err != nil {
	log.Printf("%q: %s\n", err, sqlStmt)
	return
}
// ...
```

## lib/pq: A pure Go postgres driver for Go's database/sql package

https://github.com/lib/pq

Features:

<ul>
<li>SSL</li>
<li>Handles bad connections for database/sql</li>
<li>Scan time.Time correctly (i.e. timestamp[tz], time[tz], date)</li>
<li>Scan binary blobs correctly (i.e. bytea)</li>
<li>Package for hstore support</li>
<li>COPY FROM support</li>
<li>pq.ParseURL for converting urls to connection strings for sql.Open.</li>
<li>Many libpq compatible environment variables</li>
<li>Unix socket support</li>
<li>Notifications: LISTEN/NOTIFY</li>
<li>pgpass support</li>
<li>GSS (Kerberos) auth</li>
</ul>

Get the package: <b>go get github.com/lib/pq</b>

Tutorials, guides, opinions, tips and interesting stuff in general: 

https://www.calhoun.io/connecting-to-a-postgresql-database-with-gos-database-sql-package/
<br>https://semaphoreci.com/community/tutorials/building-and-testing-a-rest-api-in-go-with-gorilla-mux-and-postgresql
<br>https://www.enterprisedb.com/postgres-tutorials/postgresql-and-golang-tutorial
<br>https://medium.com/avitotech/how-to-work-with-postgres-in-go-bad2dabd13e4
<br>https://astaxie.gitbooks.io/build-web-application-with-golang/content/en/05.4.html
<br>https://golangcode.com/postgresql-connect-and-query/

```golang
package main

import (
  "database/sql"
  "fmt"

  _ "github.com/lib/pq"
)

const (
  host     = "localhost"
  port     = 5432
  user     = "postgres"
  password = "password"
  dbname   = "demo_db"
)

// ...

func main() {
  psqlInfo := fmt.Sprintf("host=%s port=%d user=%s "+
    "password=%s dbname=%s sslmode=disable",
    host, port, user, password, dbname)
  
  db, err := sql.Open("postgres", psqlInfo)
  if err != nil {
	panic(err)
  }
  defer db.Close()
  //..
}
```

## go-pg: PostgreSQL client and ORM for Golang

https://github.com/go-pg/pg
<br>https://pg.uptrace.dev/

Features:

<ul>
<li>Basic types: integers, floats, string, bool, time.Time, net.IP, net.IPNet.</li>
<li>sql.NullBool, sql.NullString, sql.NullInt64, sql.NullFloat64 and pg.NullTime.</li>
<li>sql.Scanner and sql/driver.Valuer interfaces.</li>
<li>Structs, maps and arrays are marshalled as JSON by default.</li>
<li>PostgreSQL multidimensional Arrays using array tag and Array wrapper.</li>
<li>Hstore using hstore tag and Hstore wrapper.</li>
<li>Composite types.</li>
<li>All struct fields are nullable by default and zero values (empty string, 0, zero time, empty map or slice, nil ptr) are marshalled as SQL NULL. pg:",notnull" is used to add SQL NOT NULL constraint and pg:",use_zero" to allow Go zero values.</li>
<li>Transactions.</li>
<li>Prepared statements.</li>
<li>Notifications using LISTEN and NOTIFY.</li>
<li>Copying data using COPY FROM and COPY TO.</li>
<li>Timeouts and canceling queries using context.Context.</li>
<li>Automatic connection pooling with circuit breaker support.</li>
<li>Queries retry on network errors.</li>
<li>Working with models using ORM and SQL.</li>
<li>Scanning variables using ORM and SQL.</li>
<li>SelectOrInsert using on-conflict.</li>
<li>INSERT ... ON CONFLICT DO UPDATE using ORM.</li>
<li>Bulk/batch inserts, updates, and deletes.</li>
<li>Common table expressions using WITH and WrapWith.</li>
<li>CountEstimate using EXPLAIN to get estimated number of matching rows.</li>
<li>ORM supports has one, belongs to, has many, and many to many with composite/multi-column primary keys.</li>
<li>Soft deletes.</li>
<li>Creating tables from structs.</li>
<li>ForEach that calls a function for each row returned by the query without loading all rows into the memory.</li>
</ul>

Get the package: <b>go get github.com/go-pg/pg/v10</b>

Tutorials, guides, opinions, tips and interesting stuff in general:

https://medium.com/tunaiku-tech/go-pg-golang-postgre-orm-2618b75c0430

```golang
import (
    "fmt"

    "github.com/go-pg/pg/v10"
    "github.com/go-pg/pg/v10/orm"
)

type User struct {
    Id     int64
    Name   string
    Emails []string
}

type Story struct {
    Id       int64
    Title    string
    AuthorId int64
    Author   *User `pg:"rel:has-one"`
}

// ...

func createSchema(db *pg.DB) error {
    models := []interface{}{
        (*User)(nil),
        (*Story)(nil),
    }

    for _, model := range models {
        err := db.Model(model).CreateTable(&orm.CreateTableOptions{
            Temp: true,
        })
        if err != nil {
            return err
        }
    }
    return nil
}
// ...
```

## gorm: The fantastic ORM library for Golang

https://gorm.io/index.html
<br>https://github.com/go-gorm/gorm

Features:

<ul>
<li>Full-Featured ORM</li>
<li>Associations (has one, has many, belongs to, many to many, polymorphism, single-table inheritance)</li>
<li>Hooks (before/after create/save/update/delete/find)</li>
<li>Eager loading with Preload, Joins</li>
<li>Transactions, Nested Transactions, Save Point, RollbackTo to Saved Point</li>
<li>Context, Prepared Statement Mode, DryRun Mode</li>
<li>Batch Insert, FindInBatches, Find/Create with Map, CRUD with SQL Expr and Context Valuer</li>
<li>SQL Builder, Upsert, Locking, Optimizer/Index/Comment Hints, Named Argument, SubQuery</li>
<li>Composite Primary Key, Indexes, Constraints</li>
<li>Auto Migrations</li>
<li>Logger</li>
<li>Extendable, flexible plugin API: Database Resolver (multiple databases, read/write splitting) / Prometheus…</li>
<li>Every feature comes with tests</li>
<li>Developer Friendly</li>
</ul>

Get the package: <b>go get -u gorm.io/gorm; go get -u gorm.io/driver/sqlite</b>

Tutorials, guides, opinions, tips and interesting stuff in general:

https://blog.logrocket.com/how-to-build-a-rest-api-with-golang-using-gin-and-gorm/
<br>https://www.enterprisedb.com/postgres-tutorials/postgresql-and-golang-tutorial
<br>https://jfrog.com/blog/top-go-modules-golang-web-apis-with-gorm/
<br>https://www.cockroachlabs.com/docs/v20.2/build-a-go-app-with-cockroachdb-gorm.html
<br>https://www.mindbowser.com/golang-go-with-gorm/
<br>https://tutorialedge.net/golang/golang-orm-tutorial/
<br>https://www.golangprograms.com/golang-restful-api-using-grom-and-gorilla-mux.html

```golang
package main

import (
  "gorm.io/gorm"
  "gorm.io/driver/sqlite"
)

type Product struct {
  gorm.Model
  Code  string
  Price uint
}

func main() {
  db, err := gorm.Open(sqlite.Open("test.db"), &gorm.Config{})
  if err != nil {
    panic("failed to connect database")
  }

  // Migrate the schema
  db.AutoMigrate(&Product{})

  // Create
  db.Create(&Product{Code: "D42", Price: 100})

  // Read
  var product Product
  db.First(&product, 1) // find product with integer primary key
  db.First(&product, "code = ?", "D42") // find product with code D42

  // Update - update product's price to 200
  db.Model(&product).Update("Price", 200)
  // Update - update multiple fields
  db.Model(&product).Updates(Product{Price: 200, Code: "F42"}) // non-zero fields
  db.Model(&product).Updates(map[string]interface{}{"Price": 200, "Code": "F42"})

  // Delete - delete product
  db.Delete(&product, 1)
}
```

# NoSQL

Drivers in Go for NoSQL databases are also available as thrid-party packages that can be added as modules to your backend project. They are ussually easy to use and provided with documentation and examples to have them ready in no time in your project. Almost every developer-top NoSQL has its driver for Go so there's always a great possibility to use in your Go project a database that suits your needs. Here's a list with the most popular ones:

## mongo-go-driver: The MongoDB supported driver for Go.

https://github.com/mongodb/mongo-go-driver

Features:

<ul>
<li>BSON objects</li>
<li>Extensive oficial documentation</li>
<li>All features from MongoDB are available, data pipelines included.</li>
<li>Short learning curve (even mastering data aggregation pipelines)</li>
<li>Easy to use and test.</li>
<li>BSON tags can be added to struct fields that are mapped to database schemas</li>
<li>Built-in CRUD</li>
</ul>

Get the package: <b>go get go.mongodb.org/mongo-driver/mongo</b>

Tutorials, guides, opinions, tips and interesting stuff in general:

https://www.mongodb.com/blog/post/mongodb-go-driver-tutorial
<br>https://www.mongodb.com/blog/post/mongodb-go-driver-tutorial-part-1-connecting-using-bson-and-crud-operations
<br>https://www.mongodb.com/blog/post/quick-start-golang-mongodb-starting-and-setup
<br>https://www.mongodb.com/blog/post/quick-start-golang--mongodb--how-to-create-documents
<br>https://www.mongodb.com/blog/post/quick-start-golang--mongodb--how-to-read-documents
<br>https://www.mongodb.com/blog/post/quick-start-golang--mongodb--how-to-update-documents
<br>https://www.mongodb.com/blog/post/quick-start-golang--mongodb--how-to-delete-documents
<br>https://www.mongodb.com/blog/post/quick-start-golang--mongodb--modeling-documents-with-go-data-structures
<br>https://www.mongodb.com/blog/post/quick-start-golang--mongodb--data-aggregation-pipeline
<br>https://www.agiratech.com/blog/building-restful-api-service-golang-using-gin-gonic/

```golang
import (
    "go.mongodb.org/mongo-driver/mongo"
    "go.mongodb.org/mongo-driver/mongo/options"
    "go.mongodb.org/mongo-driver/mongo/readpref"
)

// ...
ctx, cancel := context.WithTimeout(context.Background(), 10*time.Second)
defer cancel()
client, err := mongo.Connect(ctx, options.Client().ApplyURI("mongodb://localhost:27017"))
// ...
```

## go-elasticsearch: The official Go client for Elasticsearch

Get the package: <b>go get github.com/elastic/go-elasticsearch/v8 master</b>

Tutorials, guides, opinions, tips and interesting stuff in general:

https://www.elastic.co/blog/the-go-client-for-elasticsearch-introduction
<br>https://blog.logrocket.com/using-elasticsearch-logstash-and-kibana-with-go-applications/
<br>https://kb.objectrocket.com/elasticsearch/how-to-get-elasticsearch-documents-using-golang-448
<br>https://dev.to/taqkarim/safely-construct-elasticsearch-queries-w-golang-4ff3
<br>https://www.freecodecamp.org/news/go-elasticsearch/

```golang
es, err := elasticsearch.NewDefaultClient()
if err != nil {
  log.Fatalf("Error creating the client: %s", err)
}

res, err := es.Info()
if err != nil {
  log.Fatalf("Error getting response: %s", err)
}

defer res.Body.Close()
log.Println(res)

// [200 OK] {
//   "name" : "node-1",
//   "cluster_name" : "go-elasticsearch"
// ...
```

## gocb: Couchbase Go Client

https://github.com/couchbase/gocb

This is the official Couchbase Go SDK library that allows you to connect to a Couchbase cluster from Go. It is written in pure Go, and uses the gocbcore library to handle communicating to the cluster over the Couchbase binary protocol.

Get the package: <b>go get github.com/couchbase/gocb/v2</b>

Tutorials, guides, opinions, tips and interesting stuff in general:

https://developer.couchbase.com/documentation/server/current/sdk/go/start-using-sdk.html
<br>https://docs.couchbase.com/go-sdk/current/hello-world/start-using-sdk.html
<br>https://www.thepolyglotdeveloper.com/2016/08/using-couchbase-server-golang-web-application/

## gocosmos

Go driver for Azure Cosmos DB SQL API which can be used with the standard database/sql package. A REST client for Azure Cosmos DB SQL API is also included.

Features:

-The REST client supports:

<ul>
<li>Database: Create, Get, Delete, List and change throughput.</li>
<li>Collection: Create, Replace, Get, Delete, List and change throughput.</li>
<li>Document: Create, Replace, Get, Delete, Query and List.</li>
</ul>

Get the package: <b>go get github.com/btnguyen2k/gocosmos</b>

Tutorials, guides, opinions, tips and interesting stuff in general:

https://docs.microsoft.com/en-us/azure/cosmos-db/create-mongodb-go
<br>https://medium.com/@The_Regan/how-to-use-azure-cosmos-db-with-golang-using-mongodbs-official-go-driver-ccbb5db54c46

```golang
import (
	"os"
	"database/sql"
	"github.com/btnguyen2k/gocosmos"
)

func main() {
	cosmosDbConnStr := "AccountEndpoint=https://localhost:8081/;AccountKey=<cosmosdb-account-key>"
	client, err := gocosmos.NewRestClient(nil, cosmosDbConnStr)
	if err != nil {
		panic(err)
	}

	dbSpec := gocosmos.DatabaseSpec{Id:"mydb", Ru: 400}
	result := client.CreateDatabase(dbSpec)
	if result.Error() != nil {
		panic(result.Error)
	}

	// database "mydb" has been created successfuly
}
```

# Microservices

## Gokit

https://github.com/go-kit/kit
<br>https://gokit.io/

Go kit is a programming toolkit for building microservices (or elegant monoliths) in Go. We solve common problems in distributed systems and application architecture so you can focus on delivering business value.

Go kit is a programming toolkit for building microservices in Go. It was created to solve common problems in distributed systems and applications and allow developers to focus on the business part of programming. It’s basically a group of co-related packages that, together, form a framework for constructing large SOAs. Its main transport layer is RPC (Remote Procedure Call), but it can also use JSON over HTTP and it’s easy to integrate with the most common infrastructural components, to reduce deployment friction and promote interoperability with existing systems. 

The most interesting feature in Go kit is it embracing the concept of concentric layers or onion architecture so the developer distibutes all functionalities in independent layers that can be put together as in a decorator pattern fashion. 

Goals:

<ul>
<li>Operate in a heterogeneous SOA — expect to interact with mostly non-Go-kit services</li>
<li>RPC as the primary messaging pattern</li>
<li>Pluggable serialization and transport — not just JSON over HTTP</li>
<li>Operate within existing infrastructures — no mandates for specific tools or technologies</li>
</ul>

Non-goals:

<ul>
<li>Supporting messaging patterns other than RPC (for now) — e.g. MPI, pub/sub, CQRS, etc.</li>
<li>Re-implementing functionality that can be provided by adapting existing software</li>
<li>Having opinions on operational concerns: deployment, configuration, process supervision, orchestration, etc.</li>
</ul>

Get the package: <b>go get github.com/go-kit/kit</b>

Tutorials, guides, opinions, tips and interesting stuff in general:

https://peter.bourgon.org/applied-go-kit/
<br>https://gokit.io/examples/
<br>https://danielsinnott.com/blog/26
<br>https://www.avantica.com/blog/building-microservices-with-golang-and-go-kit
<br>https://dev.to/eminetto/microservices-in-go-using-the-go-kit-jjf
<br>https://www.velotio.com/engineering-blog/build-a-containerized-microservice-in-golang

```golang
import (
	"context"
	"github.com/go-kit/kit/endpoint"
	httptransport "github.com/go-kit/kit/transport/http"
)

func makeUppercaseEndpoint(svc StringService) endpoint.Endpoint {
	return func(_ context.Context, request interface{}) (interface{}, error) {
		req := request.(uppercaseRequest)
		v, err := svc.Uppercase(req.S)
		if err != nil {
			return uppercaseResponse{v, err.Error()}, nil
		}
		return uppercaseResponse{v, ""}, nil
	}
}

func makeCountEndpoint(svc StringService) endpoint.Endpoint {
	return func(_ context.Context, request interface{}) (interface{}, error) {
		req := request.(countRequest)
		v := svc.Count(req.S)
		return countResponse{v}, nil
	}
}

// ...
func main() {
	svc := stringService{}

	uppercaseHandler := httptransport.NewServer(
		makeUppercaseEndpoint(svc),
		decodeUppercaseRequest,
		encodeResponse,
	)

	countHandler := httptransport.NewServer(
		makeCountEndpoint(svc),
		decodeCountRequest,
		encodeResponse,
	)

	http.Handle("/uppercase", uppercaseHandler)
	http.Handle("/count", countHandler)
	log.Fatal(http.ListenAndServe(":8080", nil))
}
```

## Gizmo: Microservice Toolkit

This toolkit provides packages to put together server and pubsub daemons with the following features:

<ul>
<li>Standardized configuration and logging</li>
<li>Health check endpoints with configurable strategies</li>
<li>Configuration for managing pprof endpoints and log levels</li>
<li>Basic interfaces to define expectations and vocabulary</li>
<li>Structured logging containing basic request information</li>
<li>Useful metrics for endpoints</li>
<li>Graceful shutdowns</li>
</ul>

Get the package: <b>go get github.com/nytimes/gizmo/</b>

Tutorials, guides, opinions, tips and interesting stuff in general:

https://open.blogs.nytimes.com/2015/12/17/introducing-gizmo/
<br>https://www.infoq.com/news/2016/01/gizmo-microservices-toolkit/

## go-micro: framework for distributed systems development.

https://github.com/asim/go-micro

Go Micro provides the core requirements for distributed systems development including RPC and Event driven communication. The Micro philosophy is sane defaults with a pluggable architecture. We provide defaults to get you started quickly but everything can be easily swapped out.

Go Micro abstracts away the details of distributed systems. Here are the main features:

<ul>
<li>Authentication</li>
<li>Dynamic config</li>
<li>Data storage</li>
<li>Service discovery</li>
<li>Load balancing</li>
<li>Message encoding</li>
<li>RPC client/server</li>
<li>Async messaging</li>
<li>Synchronization</li>
<li>Pluggable interfaces</li>
</ul>

Get the package: <b>go get github.com/micro/micro/v3</b>

Tutorials, guides, opinions, tips and interesting stuff in general:

https://medium.com/microhq/writing-microservices-with-go-micro-b6e0880b8829
<br>https://itnext.io/micro-in-action-getting-started-a79916ae3cac
<br>https://itnext.io/go-micro-on-kubernetes-getting-started-2959dd6c2d4f
<br>https://winderresearch.com/go-micro-opinions-and-examples/
<br>https://andrehonsberg.io/2020/08/go-micro-v3-installation-setup-and-notes/
<br>https://andrehonsberg.io/2020/08/golang-microservices-go-micro/
<br>https://jaxenter.com/go-micro-microservice-framework-154603.html

```golang
import "github.com/asim/go-micro/v3"

// create a new service
service := micro.NewService(
    micro.Name("helloworld"),
)

// initialise flags
service.Init()

// start the service
service.Run()
```

# AWS lambda

AWS Lambda is a serverless compute service that runs your code in response to events and automatically manages the underlying compute resources for you. You can use AWS Lambda to extend other AWS services with custom logic, or create your own back-end services that operate at AWS scale, performance, and security. AWS Lambda can automatically run code in response to multiple events, such as HTTP requests via Amazon API Gateway, modifications to objects in Amazon S3 buckets, table updates in Amazon DynamoDB, and state transitions in AWS Step Functions.

## What is a Lambda function?

The code you run on AWS Lambda is called a “Lambda function.” After you create your Lambda function it is always ready to run as soon as it is triggered, similar to a formula in a spreadsheet. Each function includes your code as well as some associated configuration information, including the function name and resource requirements. Lambda functions are “stateless,” with no affinity to the underlying infrastructure, so that Lambda can rapidly launch as many copies of the function as needed to scale to the rate of incoming events. The main features are:

<ul>
<li>Extend other AWS services with custom logic.</li>
<li>Build custom back-end services.</li>
<li>Bring your own code.</li>
<li>Completely automated administration.</li>
<li>Built-in fault tolerance.</li>
<li>Automatic scaling.</li>
<li>Connect to relational databases.</li>
<li>Fine grained control over performance.</li>
<li>Connect to shared file systems.</li>
<li>Orchestrate multiple functions.</li>
<li>Integrated security model.</li>
<li>Only pay for what you use.</li>
</ul>

## Frameworks or tools

The following list is a reference to well-known lambda frameworks very popular and widely used by software developers that choose lambdas for their backends.

### Serverless

https://www.serverless.com
<br>https://github.com/serverless/serverless

The Serverless Framework – Build applications comprised of microservices that run in response to events, auto-scale for you, and only charge you when they run. This lowers the total cost of maintaining your apps, enabling you to build more logic, faster.

The Framework uses new event-driven compute services, like AWS Lambda, Google Cloud Functions, and more. It's a command-line tool, providing scaffolding, workflow automation and best practices for developing and deploying your serverless architecture. It's also completely extensible via plugins.

### Features

<ul>
<li>Supports Node.js, Python, Java, Go, C#, Ruby, Swift, Kotlin, PHP, Scala, & F#</li>
<li>Manages the lifecycle of your serverless architecture (build, deploy, update, delete)</li>
<li>Safely deploy functions, events and their required resources together via provider resource managers (e.g., AWS CloudFormation)</li>
<li>Functions can be grouped ("serverless services") for easy management of code, resources & processes, across large projects & teams</li>
<li>Minimal configuration and scaffolding</li>
<li>Built-in support for multiple stages</li>
<li>Optimized for CI/CD workflows</li>
<li>Loaded with automation, optimization and best practices</li>
<li>100% Extensible: Extend or modify the Framework and its operations via Plugins</li>
<li>An ecosystem of serverless services and plugins</li>
<li>A passionate and welcoming community!</li>
</ul>

1. Install via npm:
```
npm install -g serverless
```

2. Set-up your Provider Credentials.

3. Create a Service:

```
# Create a new Serverless Service/Project
serverless create --template aws-nodejs --path my-service
# Change into the newly created directory
cd my-service
```

4. Deploy a Service:

```
serverless deploy -v
```


### SAM

https://aws.amazon.com/serverless/sam/
<br> https://github.com/aws/serverless-application-model

The AWS Serverless Application Model (SAM) is an open-source framework for building serverless applications. It provides shorthand syntax to express functions, APIs, databases, and event source mappings. With just a few lines of configuration, you can define the application you want and model it.

### Why SAM

<ul>
<li>Single-deployment configuration</li>
<li>Local debugging and testing</li>
<li>Deep integration with development tools.
    <ul>
        <li>IDEs: PyCharm, IntelliJ, Visual Studio Code, Visual Studio, AWS Cloud9</li>
        <li>Build: CodeBuild</li>
        <li>Deploy: CodeDeploy, Jenkins</li>
        <li>Continuous Delivery Pipelines: CodePipeline</li>
        <li>Discover Serverless Apps & Patterns: AWS Serverless Application Repository</li>
    </ul>
</li>
<li>Built-in best practices</li>
<li>Extension of AWS CloudFormation</li>
</ul>

### Terraform

www.terraform.io <br>
https://github.com/hashicorp/terraform

Terraform is a tool for building, changing, and versioning infrastructure safely and efficiently. Terraform can manage existing and popular service providers as well as custom in-house solutions. The key features of Terraform are:

<ul>
<li>Infrastructure as Code</li>
<li>Execution Plans</li>
<li>Resource Graph</li>
<li>Change Automation</li>
</ul>

# Message brokers

A message broker is software that enables applications, systems, and services to communicate with each other and exchange information. The message broker does this by translating messages between formal messaging protocols. This allows interdependent services to “talk” with one another directly, even if they were written in different languages or implemented on different platforms.

## Apache Kafka

https://kafka.apache.org <br>
https://github.com/confluentinc/confluent-kafka-go

Apache Kafka is an open-source distributed event streaming platform used by thousands of companies for high-performance data pipelines, streaming analytics, data integration, and mission-critical applications.

```golang
import (
    "fmt"
    "gopkg.in/confluentinc/confluent-kafka-go.v1/kafka"
)

func main() {

    c, err := kafka.NewConsumer(&kafka.ConfigMap{
        "bootstrap.servers": "localhost",
        "group.id":          "myGroup",
        "auto.offset.reset": "earliest",
    })

    if err != nil {
        panic(err)
    }

    c.SubscribeTopics([]string{"myTopic", "^aRegex.*[Tt]opic"}, nil)

    for {
        msg, err := c.ReadMessage(-1)
        if err == nil {
            fmt.Printf("Message on %s: %s\n", msg.TopicPartition, string(msg.Value))
        } else {
            // The client will automatically try to recover from all errors.
            fmt.Printf("Consumer error: %v (%v)\n", err, msg)
        }
    }

    c.Close()
}

```

### Core Capabilities

<ul>
<li>High Throughput</li>
<li>Scalable</li>
<li>Permanent storage</li>
<li>High availability</li>
</ul>

### Ecosystem

<ul>
<li>Built-in Stream Processing</li>
<li>Connect To Almost Anything</li>
<li>Client Libraries</li>
<li>Large Ecosystem Open Source Tools </li>
</ul>

## RabbitMQ

https://www.rabbitmq.com <br>
https://github.com/streadway/amqp

RabbitMQ is the most widely deployed open source message broker. With tens of thousands of users, RabbitMQ is one of the most popular open source message brokers. From T-Mobile to Runtastic, RabbitMQ is used worldwide at small startups and large enterprises.

### Features

<ul>
<li>Asynchronous Messaging</li>
<li>Developer Experience</li>
<li>Distributed Deployment</li>
<li>Enterprise & Cloud Ready</li>
<li>Tools & Plugins</li>
<li>Management & Monitoring</li>
</ul>

1. Connect to RabbitMQ server
```golang
conn, err := amqp.Dial("amqp://guest:guest@localhost:5672/")
failOnError(err, "Failed to connect to RabbitMQ")
defer conn.Close()
```

2. Next we create a channel
```golang
ch, err := conn.Channel()
failOnError(err, "Failed to open a channel")
defer ch.Close()
```

3. To send, we must declare a queue for us to send to; then we can publish a message to the queue:
```golang
q, err := ch.QueueDeclare(
    "hello", // name
    false,   // durable
    false,   // delete when unused
    false,   // exclusive
    false,   // no-wait
    nil,     // arguments
)
failOnError(err, "Failed to declare a queue")

body := "Hello World!"
err = ch.Publish(
    "",     // exchange
    q.Name, // routing key
    false,  // mandatory
    false,  // immediate
    amqp.Publishing {
    ContentType: "text/plain",
    Body:        []byte(body),
})
failOnError(err, "Failed to publish a message")
```

# What is memory caching?

Memory caching (often simply referred to as caching) is a technique in which computer applications temporarily store data in a computer’s main memory (i.e., random access memory, or RAM) to enable fast retrievals of that data. The RAM that is used for the temporary storage is known as the cache. Since accessing RAM is significantly faster than accessing other media like hard disk drives or networks, caching helps applications run faster due to faster access to data.

## Redis

https://redis.io/ <br>
https://github.com/go-redis/redis

Redis is an open source (BSD licensed), in-memory data structure store, used as a database, cache, and message broker. Redis provides data structures such as strings, hashes, lists, sets, sorted sets with range queries, bitmaps, hyperloglogs, geospatial indexes, and streams. Redis has built-in replication, Lua scripting, LRU eviction, transactions, and different levels of on-disk persistence, and provides high availability via Redis Sentinel and automatic partitioning with Redis Cluster

### Features

<ul>
<li>Automatic connection pooling with circuit breaker support</li>
<li>Pub/Sub</li>
<li>Transactions</li>
<li>Pipeline and TxPipeline</li>
<li>Scripting</li>
<li>Timeouts</li>
<li>Redis Sentinel</li>
<li>Redis Cluster</li>
</ul>

```golang
import (
    "context"
    "github.com/go-redis/redis/v8"
)

var ctx = context.Background()

func ExampleClient() {
    rdb := redis.NewClient(&redis.Options{
    Addr:     "localhost:6379",
    Password: "", // no password set
    DB:       0,  // use default DB
})

err := rdb.Set(ctx, "key", "value", 0).Err()
if err != nil {
    panic(err)
}

val, err := rdb.Get(ctx, "key").Result()
if err != nil {
    panic(err)
}
fmt.Println("key", val)

val2, err := rdb.Get(ctx, "key2").Result()
if err == redis.Nil {
    fmt.Println("key2 does not exist")
} else if err != nil {
    panic(err)
} else {
    fmt.Println("key2", val2)
}
// Output: key value
// key2 does not exist
}
```

## Memcached

https://memcached.org/ <br>
https://github.com/bradfitz/gomemcache

Free & open source, high-performance, distributed memory object caching system, generic in nature, but intended for use in speeding up dynamic web applications by alleviating database load.

<ul>
<li>Simple Key/Value Store</li>
<li>Logic Half in Client, Half in Server</li>
<li>Servers are Disconnected From Each Other</li>
<li>O(1)</li>
<li>Forgetting is a Feature</li>
</ul>

```golang
import (
"github.com/bradfitz/gomemcache/memcache"
)

func main() {
    mc := memcache.New("10.0.0.1:11211", "10.0.0.2:11211", "10.0.0.3:11212")
    mc.Set(&memcache.Item{Key: "foo", Value: []byte("my value")})

    it, err := mc.Get("foo")
    ...
}

```