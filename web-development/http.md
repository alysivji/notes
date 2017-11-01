## [HTTP Messages](https://developer.mozilla.org/en-US/docs/Web/HTTP/Messages)

> HTTP messages are how data is exchanged between a server and a client. There are two types of messages: *requests* sent by the client to trigger an action on the server, and *responses*, the answer from the server.

* Messages are text encoded in ASCII

> HTTP requests, and responses, share similar structure and are composed of:
> 1. A start-line describing the requests to be implemented, or its status of whether successful or a failure. This start-line is always a single line.
> 1. An optional set of HTTP headers specifying the request, or describing the body included in the message.
> 1. A blank line indicating all meta-information for the request have been sent.
> 1. An optional body containing data associated with the request (like content of an HTML form), or the document associated with a response. The presence of the body and its size is specified by the start-line and HTTP headers.

### HTTP Request

> HTTP requests are messages sent by the client to initiate an action on the server.

#### Start line

Contains three elements:
1. **HTTP method** - verb or noun that describes the action to be performed
2. **Request target** - URL or absolute path of resource
3. **HTTP version** - defines structure for rest of the reamining message and indicates the expected version to use for response

#### Headers

```Header: value```

Different types of headers:
* **General headers** - apply to message as whole
* **Request headers** - give request context or conditionally restrict it
* **Entity headers** - applies to the body of teh request (i.e. Content-Type)

#### Body

Not all requests have a body, but if we need to send data to the serve this is where it goes

Two types:
1. **Single-resource bodies** - Single file defined by two headers (```Content-Type``` and ```Content-Length```)
1. **Multiple-resource bodies** - Multiparty body each containing different kind of information, associated with HTML Forms.

### HTTP Responses

#### Status line

Contains:
1.  **Protocol version**
1. **Status code** - indiciation success or failure of request
1. **Status text** - Brief textual description of status code

#### Headers

```Header: value```

Can be divided into several groups:
* **General headers** - apply to whole message
* **Response headers** - give additional information about server
* **Entity headers** - apply to the body of the request

#### Body

Broadly divided into three categories:
1. **Single resource bodies of known length** - Defined by two headers (```Content-Type``` and ```Content-Length```)
2. **Single resource bodies of unknown length** - Encoded with chunks (```Transfer-Encoding:chunked```)
3. **Multiple resource bodies** - Consist of a multipart body each with different inforation

### HTTP/2 Frames

* HTTP/2 introduces an extra step into the workflow, it divides the HTTP/1.x messages into frames which are then streamed.
* Works when both browser and server have HTTP2 enabled.

## [A Typical HTTP Session](https://developer.mozilla.org/en-US/docs/Web/HTTP/Session)

> Client-server sessions consist of three phases:
> 1. The client establishes a TCP connection
> 1. The client sends its request, and waits for the answer.
> 1. The server processes the request, sending back its answer, providing a status code and appropriate data.

> As of HTTP/1.1, the connection is no longer closed after completing the third phase, and the client is now granted a further request: this means the second and third phases can now be performed any number of times.

### Establishing a Connection

* Client establishes a connection by initating a connection in the underlying transport layer

### Sending a client request

The user-agent can send the request consisting of text directives, separated by  CRLF (carriage return, followed by line feed), divided into three blocks:
1. First line contains request method and parameters (absolute path, HTTP protocol version)
1. Subsequent lines contain HTTP header
1. Optional data block

### Server response

The web server processes the request and returns a reponse consisting of text directives, separated by  CRLF (carriage return, followed by line feed), divided into three blocks:
1. Status line consisting of an acknowledgement of HTTP protocol, followed by a status request, and its meaning
1. Subsequent lines contain HTTP header
1. Optional data block
