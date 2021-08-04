# Time to practice...

## 1. Chat app

Socket.io ([here](https://socket.io/) and [there](https://github.com/googollee/go-socket.io)) is a third-party package for real-time, bidirectional communication with sockets. Use this package and other features in Go's standard library to implement a simple (tiny) chat application. It's up to you how the chat app will look like.

MVP:
* All users' messages can be broadcasted in the chat room.
* Messages have to be logged in a file.
* Messages should be prefixed with date and time

## 2. TCP server/client app

Implement an inter-process communication using TCP binding (do not use HTTP!). The app implements a client/server communication using TCP protocol. You are asked to deploy both the client and the server. Suggested packages: log, net, sync, bytes, bufio, io, time, fmt.

MVP:
* Implement a server application that listens to any port of your choice for incomming connections. The server receives message strings delimited by the new line character ('\n') and responds to the client with the same string uppercased. The server can be asked by the client to close the connection with a "quit!" command.
* Implement a client application that captures the console input and sends it to the server. The client connects to the server's port and keeps waiting for user input to send to the server. The client can send a "quit!" command to close the connection.
* Both the client and server application should log either communication events and errors that happen during execution.