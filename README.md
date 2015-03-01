# AwesomeSockets
A simple java library to make server-socket communications less painful.

## Introduction

I was learning about servers and sockets in  *50.003: Elements of Software Construction*, when I received a problem set with many instances when the simplest solution was to reuse code for starting the servers and sockets.

This was incredibly painful and verbose. After opening a server and accepting a client, we have to write the frustrating syntax to write and read input:

```java
PrintWriter out = new PrintWriter(clientSocket.getOutputStream(), true);     
out.println(stdIn.readLine());
```

Or to read input from the client.

```java
BufferedReader in = new BufferedReader(
                new InputStreamReader(
                                clientSocket.getInputStream()));
```

This is further exacerbated when we wish to extend our server to read inputs from multiple clients with different communication protocols, and such syntax simply makes code unnecessarily messay and difficult to read. I wanted to create easier Server and Client objects that acts as a wrapper for the native java `java.net.ServerSocket;` and `java.net.Socket;`, which will maintain the features of the native objects as I need them.

## How to Use
With AwesomeSockets, all that has to be done is create a new socket,
```java
AwesomeServerSocket awesomeServer = new AwesomeServerSocket(4321);
awesomeServer.debugMode = false;
```

Setting `debugMode` to false will show print statements that indicate the state of the server. Now all we have to do is to call `accept` on the server as many times as we want clients to be registered.

```java
awesomeServer.acceptClient(soTimeOut); 
```

The optional `soTimeOut` affects the timeout for read message calls on the accepted client. 

Now sending and reading a message to the server would be as easy as:

```java
awesomeServer.sendMessageLineForClient(index,  message);
String messageReceived = awesomeServer.readMessageLineForClient(index);
```

We can broadcast messages to all clients through: 

```java
awesomeServer.sendMessageToAllClients(message);
```

And it even supports multi-threaded reading of messages from any of the clients, through an extensible `ClientListener`.


```java
awesomeServer.startAllClientListenersForListener(DefaultClientListenerRunnable.class);
```

Similarly for the server, simply call:

```java
AwesomeClientSocket awesomeClient = new AwesomeClientSocket("localhost", 4321);
awesomeClient.sendMessageLine(messageToServer); // send message
String receivedMessage = awesomeClient.readMessageLine() // read message
```
## To Do

Streamline the extension of the `ClientListener`, into an interface or an abstract class to make subclassing easier.

## Future
As and when I encounter new needs for certain functions of servers and sockets, I intend to modify this class to make it as general as possible.
