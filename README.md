# Web Sockets

## Problem Due to Which WebSockets Are Used

Traditional HTTP protocol follows a request-response model where the client requests data from the server, and the server responds. While this model works well for many applications, it has limitations, particularly in scenarios requiring real-time, bidirectional communication. Some of the key issues include:

### 1. Latency and Inefficiency
- **Polling**: Many real-time applications resort to polling, where the client repeatedly sends requests to the server to check for updates. This approach leads to increased latency and inefficiency due to constant server hits and redundant data transfer.
- **Long Polling**: An improvement over regular polling, long polling keeps the connection open until the server has new information to send. However, it still involves significant overhead and can strain server resources.

### 2. Lack of Real-Time Interaction
- **Delayed Updates**: Applications requiring instant updates, such as chat applications, online gaming, and live notifications, suffer from delayed data delivery, leading to a poor user experience.
- **Server Push Limitations**: Traditional HTTP lacks a native mechanism for the server to push updates to the client without a client request, further limiting real-time interaction.

### 3. Scalability Challenges
- **Resource Consumption**: Continuous polling or long polling can consume considerable server and network resources, making it challenging to scale applications efficiently.
- **Connection Management**: Managing numerous short-lived HTTP connections can be cumbersome and resource-intensive, affecting the application's scalability and performance.

<div align="center">
<img src="https://i2.wp.com/4.bp.blogspot.com/-Lsa7ViVflzM/XLoJWJqr1GI/AAAAAAAAAV0/Z18R2se-Qo46MoZMOm_oTNvm7sppxVqwwCLcBGAs/s1600/Polling.jpg?ssl=1" >
</div>

WebSockets address these problems by providing a full-duplex communication channel over a single, long-lived connection. This allows for real-time, bidirectional data exchange between the client and server, significantly reducing latency, improving efficiency, and enhancing the user experience in applications requiring real-time interaction.


<br>

---

<br>


## WebSocket Connection Overview

WebSockets address these problems by providing a way to open a persistent connection between a client (like a web browser) and a server, allowing for real-time data exchange. Unlike traditional HTTP requests, where a new connection is made for each request, WebSockets allow for continuous communication without the overhead of establishing a new connection each time.


## How to Establish a WebSocket Connection

To create a WebSocket connection, the client sends an HTTP request to the server that includes specific headers indicating that it wants to upgrade the connection to a WebSocket. Here’s how it works:

### 1. WebSocket Request

Here is an example of a WebSocket request:

```http
GET /chat HTTP/1.1
Host: example.com
Upgrade: websocket
Connection: Upgrade
Sec-WebSocket-Key: dGhlIHNhbXBsZSBub25jZQ==
Sec-WebSocket-Version: 13
```

#### Breakdown of the Request:

- **GET /chat HTTP/1.1**: 
  - `GET`: This is the HTTP method used to request data from the server.
  - `/chat`: This is the endpoint where the WebSocket connection will be established. It could be any route defined on the server.
  - `HTTP/1.1`: The version of HTTP being used.

- **Host**: 
  - This specifies the domain name of the server (e.g., `example.com`) that the client is trying to connect to.

- **Upgrade**: 
  - This header indicates that the client wants to switch from the HTTP protocol to the WebSocket protocol.

- **Connection**: 
  - Must be set to `Upgrade`, which tells the server that the client wants to upgrade the connection.

- **Sec-WebSocket-Key**: 
  - This is a randomly generated base64-encoded string unique to the WebSocket request. The server will use this key to create a response and validate the connection.

- **Sec-WebSocket-Version**: 
  - Indicates the version of the WebSocket protocol the client is requesting (usually set to `13`, which is the latest version).

### 2. WebSocket Response

If the server supports the WebSocket connection, it responds with a status code indicating that the protocol upgrade was successful:

```http
HTTP/1.1 101 Switching Protocols
Upgrade: websocket
Connection: Upgrade
Sec-WebSocket-Accept: dGhlIHNhbXBsZSBub25jZQ==
```

#### Breakdown of the Response:

- **HTTP/1.1 101 Switching Protocols**: 
  - This response code indicates that the server has accepted the request to switch to the WebSocket protocol.

- **Upgrade**: 
  - Confirms that the server is switching to the WebSocket protocol.

- **Connection**: 
  - Confirms the upgrade to WebSocket.

- **Sec-WebSocket-Accept**: 
  - This value is generated by the server using the `Sec-WebSocket-Key` provided by the client. It helps ensure the security of the connection and is also base64-encoded.

### Example of a Complete WebSocket Handshake

Here's a simplified flow of how a WebSocket handshake occurs:

1. **Client initiates a connection** by sending the WebSocket request to the server.
2. **Server receives the request** and checks if it can accept the WebSocket connection.
3. **Server sends back the response** confirming the connection upgrade.
4. **Both the client and server can now send and receive messages** over this persistent connection.

