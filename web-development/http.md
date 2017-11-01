# [HTTP: Hypertext Transfer Protocol](https://developer.mozilla.org/en-US/docs/Web/HTTP)

* [Application layer](https://en.wikipedia.org/wiki/Application_layer) protocol for transmitting hypermedia document
  * Application layer => abstraction layer containing communication protocols and interface methods

* Follows [client-server model](https://en.wikipedia.org/wiki/Client%E2%80%93server_model)
	* client opens a connection to make a request
	* waits until it recieves a response

* HTTP is [stateless](https://en.wikipedia.org/wiki/Stateless_protocol)
	* no information about the communication that occured is retained by either sender or reciver
	* Pros: No need to keep track of what happened, lost connections are not a problem
	* Cons: Sending extra information in each request that needs to be interpreted by the server

## [An Overview of HTTP](https://developer.mozilla.org/en-US/docs/Web/HTTP/Overview)

* Basic idea:
	* a web document is made up of many different sub-documents
	* when a web browser goes to a page, it starts to fetch all the resources needed
	* once all the information is downloaded, the web browser can render the document as described

**requests** - messages sent by the client
**response** - messages sent by the server as an answer

* Workflow:
	* *user-agent* sends a <u>request</u>
	* server processes request, sends back a <u>response</u>

> To present a Web page, the browser sends an original request to fetch the HTML document from the page. It then parses this file, fetching additional requests corresponding to execution scripts, layout information (CSS) to display, and sub-resources contained within the page (usually images and videos). The Web browser then mixes these resources to present to the user a complete document, the Web page. Scripts executed by the browser can fetch more resources in later phases and the browser updates the Web page accordingly.

* To the client, the server looks like a single machine. It "may actually be a collection of servers, sharing the load (load balancing) or a complex piece of software interrogating other computers (like cache, a DB server, e-commerce servers, …), totally or partially generating the document on demand."
	* Abstration is cool!

* Proxies relay the HTTP messages between the client and server. Proxies can perform several functions such as:
	* caching (the cache can be public or private, like the browser cache)
	* filtering (like an antivirus scan, parental controls, …)
	* load balancing (to allow multiple servers to serve the different requests)
	* authentication (to control access to different resources)
	* logging (allowing the storage of historical information)

* Note: Proxies must not alter requests methods, but they have the ability to change some headers. [More info](https://stackoverflow.com/questions/10369679/do-http-proxy-servers-modify-request-packets).

> HTTP is stateless: there is no link between two requests being successively carried out on the same connection. This immediately has the prospect of being problematic for users attempting to interact with certain pages coherently, for example, using e-commerce shopping baskets. But while the core of HTTP itself is stateless, HTTP cookies allow the use of stateful sessions. Using header extensibility, HTTP Cookies are added to the workflow, allowing session creation on each HTTP request to share the same context, or the same state.

### HTTP Messages

#### Request

>Requests consists of the following elements:
> * An HTTP method, usually a verb like GET, POST or a noun like OPTIONS or HEAD that defines the operation the client wants to perform. Typically, a client wants to fetch a resource (using GET) or post the value of an HTML form (using POST), though more operations may be needed in other cases.
> * The path of the resource to fetch; the URL of the resource stripped from elements that are obvious from the context, for example without the protocol (http://), the domain (here developer.mozilla.org), or the TCP port (here 80).
> * The version of the HTTP protocol.
> * Optional headers that convey additional information for the servers.
> * Or a body, for some methods like POST, similar to those in responses, which contain the resource sent.

Example Request
```text
GET / HTTP/1.1
Host: developer.mozilla.org
Accept-Language: fr
```

#### Response

> Responses consist of the following elements:
> * The version of the HTTP protocol they follow.
> * A status code, indicating if the request has been successful, or not, and why.
> * A status message, a non-authoritative short description of the status code.
> * HTTP headers, like those for requests.
> * Optionally, a body containing the fetched resource.

Example Response
```text
HTTP/1.1 200 OK
Date: Sat, 09 Oct 2010 14:28:02 GMT
Server: Apache
Last-Modified: Tue, 01 Dec 2009 20:18:22 GMT
ETag: "51142bc1-7449-479b075b2891b"
Accept-Ranges: bytes
Content-Length: 29769
Content-Type: text/html

<!DOCTYPE html... (here comes the 29769 bytes of the requested web page)
```

## [HTTP caching](https://developer.mozilla.org/en-US/docs/Web/HTTP/Caching)

> Caching is a technique that stores a copy of a given resource and serves it back when requested. When a web cache has a requested resource in its store, it intercepts the request and returns its copy instead of re-downloading from the originating server. This achieves several goals: it eases the load of the server that doesn’t need to serve all clients itself, and it improves performance by being closer to the client, i.e., it takes less time to transmit the resource back. For a web site, it is a major component in achieving high performance. On the other side, it has to be configured properly as not all resources stay identical forever: it is important to cache a resource only until it changes, not longer.

* [```Cache-control```](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Cache-Control) is used in both requests and responses to specify directives (i.e. what can cache, for how long) for caching in both requests and responses

* [304](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/304) (Not Modified) is what is sent when a cache wants to know if the requested resource is still "fresh"

## [HTTP Cookies](https://developer.mozilla.org/en-US/docs/Web/HTTP/Cookies)

> An HTTP cookie (web cookie, browser cookie) is a small piece of data that a server sends to the user's web browser. The browser may store it and send it back with the next request to the same server. Typically, it's used to tell if two requests came from the same browser — keeping a user logged-in, for example. It remembers stateful information for the stateless HTTP protocol.
>
> Cookies are mainly used for three purposes:
> * **Session management** - Logins, shopping carts, game scores, or anything else the server should remember
> * **Personalization** - User preferences, themes, and other settings
> * **Tracking** - Recording and analyzing user behavior

* Cookies used to be used for storing client data, but now the recommended practice is to use [Web Storage API](https://developer.mozilla.org/en-US/docs/Web/API/Web_Storage_API) or [IndexedDB API](https://developer.mozilla.org/en-US/docs/Web/API/IndexedDB_API)
	* TODO

* HttpOnly cookie attribute can help to mitigate cross-site scripting attacks by preventing access to cookie value through JavaScript

### Creating Cookies

> When receiving an HTTP request, a server can send a Set-Cookie header with the response. The cookie is usually stored by the browser, and then the cookie is sent with requests made to the same server inside a Cookie HTTP header. An expiration date or duration can be specified, after which the cookie is no longer sent. Additionally, restrictions to a specific domain and path can be set, limiting where the cookie is sent.

Example Response
```text
HTTP/1.0 200 OK
Content-type: text/html
Set-Cookie: yummy_cookie=choco
Set-Cookie: tasty_cookie=strawberry

[page content]
```

Subsequent Request would be of the form:
```text
GET /sample_page.html HTTP/1.1
Host: www.example.org
Cookie: yummy_cookie=choco; tasty_cookie=strawberry
```

### Types of Cookies

**Session cookie** - deleted when the client shuts down
**Permanent cookie** - can set ```Expires``` or ```Max-Age``` to have it persist when client closes
**Secure and HTTPOnly cookies** - only sent to the server with an encyrpted request over HTTPS

**[Zombie cookie](https://en.wikipedia.org/wiki/Zombie_cookie)** - HTTP cookie that is recreated after deletion

### Scope of cookies

> The ```Domain``` and ```Path``` directives define the scope of the cookie: what URLs the cookies should be sent to.

## [Cross-Origin Resource Sharing (CORS)](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS)

>Cross-Origin Resource Sharing (CORS) is a mechanism that uses additional HTTP headers to let a user agent gain permission to access selected resources from a server on a different origin (domain) than the site currently in use. A user agent makes a cross-origin HTTP request when it requests a resource from a different domain, protocol, or port than the one from which the current document originated.
>
> An example of a cross-origin request: A HTML page served from http://domain-a.com makes an <img> src request for http://domain-b.com/image.jpg. Many pages on the web today load resources like CSS stylesheets, images, and scripts from separate domains, such as content delivery networks (CDNs).

* TODO: Read more as needed

## [Evolution of HTTP](https://developer.mozilla.org/en-US/docs/Web/HTTP/Basics_of_HTTP/Evolution_of_HTTP)

Great read.

## [Security Guidelines](https://wiki.mozilla.org/Security/Guidelines/Web_Security)

Lots of tips.

TODO: work as you launch a website

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
