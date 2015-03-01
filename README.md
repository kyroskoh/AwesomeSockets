# AwesomeSockets
A java library to make server-socket communications less painful.

## Introduction

Writing client server code in Java is more often than not, annoying and excessively verbose. To open and accept a server in Java, we have to do:

```java
ServerSocket serverSocket = new ServerSocket(4321);
Socket clientSocket = serverSocket.accept();```

The most frustrating part is to write the syntax to write input to the client:

```java
PrintWriter out =
                new PrintWriter(clientSocket.getOutputStream(), true);     
                out.println(stdIn.readLine());
                ```

Or to read input from the client.

```java
BufferedReader in = new BufferedReader(
                new InputStreamReader(clientSocket.getInputStream()));
                       ```

This syntax is often unnecessary, and when we wish to extend our server to read inputs from multiple clients, we have to go through this tedious syntax again.

## How to Use
With AwesomeSockets, all that has to be done is create a new socket,
```java
AwesomeServerSocket awesomeServer = new AwesomeServerSocket(4321);
awesomeServer.debugMode = false;```

Setting `debugMode` to false will show print statements that indicate the state of the server. Now all we have to do is to call `accept` on the server as many times as we want clients to be registered.

```java
awesomeServer.acceptClient(soTimeOut); ```

The optional `soTimeOut` affects the timeout for read message calls on the accepted client. 

Now sending and reading a message to the server would be as easy as:

```java
awesomeServer.sendMessageLineForClient(index,  message);
String messageReceived = awesomeServer.readMessageLineForClient(index);```

We can broadcast messages to all clients through: 

```java
awesomeServer.sendMessageToAllClients(message);```

And it even supports multi-threaded reading of messages from any of the clients, through an extensible `clientListener`.


```java
awesomeServer.startAllClientListenersForListener(DefaultClientListenerRunnable.class);```

Similarly for the server, simply call

```java
AwesomeClientSocket awesomeClient = new AwesomeClientSocket("localhost", 4321);
awesomeClient.sendMessageLine(messageToServer); // send message
String receivedMessage = awesomeClient.readMessageLine() // read message```
