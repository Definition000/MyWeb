HTTP is a TCP/IP-based application layer communication protocol that standardizes how clients and servers communicate with each other. It defines how content is requested and transmitted across the internet.
## The History
### HTTP/0.9 - The One Liner (1991)
The first documented version of HTTP was [HTTP/0.9](https://www.w3.org/Protocols/HTTP/AsImplemented.html) which was put forward in 1991.
- No headers
- `GET` was the only allowed method
- Response had to be HTML
### HTTP/1.0 - 1996
In 1996, the next version of HTTP i.e. HTTP/1.0 evolved that vastly improved over the original version.
Here is how a sample HTTP/1.0 request and response might have looked like:

```
GET / HTTP/1.0
Host: cs.fyi
User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10_10_5)
Accept: */*
```
Example response to the request above may have looked like below

```
HTTP/1.0 200 OK 
Content-Type: text/plain
Content-Length: 137582
Expires: Thu, 05 Dec 1997 16:00:00 GMT
Last-Modified: Wed, 5 August 1996 15:55:28 GMT
Server: Apache 0.84

(response body)
(connection closed)
```

#### Three-way Handshake

Three-way handshake in it’s simples form is that all the TCP connections begin with a three-way handshake in which the client and the server share a series of packets before starting to share the application data.

- SYN - Client picks up a random number, let’s say x, and sends it to the server.
- SYN ACK - Server acknowledges the request by sending an ACK packet back to the client which is made up of a random number, let’s say y picked up by server and the number x+1 where x is the number that was sent by the client
- ACK - Client increments the number y received from the server and sends an ACK packet back with the number y+1
### HTTP/1.1
 The major improvements over HTTP/1.0 included
- New HTTP methods were added, which introduced PUT, PATCH, OPTIONS, DELETE
- Hostname Identification In HTTP/1.0 Host header wasn’t required but HTTP/1.1 made it required.
- Persistent Connections As discussed above, in HTTP/1.0 there was only one request per connection and the connection was closed as soon as the request was fulfilled which resulted in accute performance hit and latency problems. HTTP/1.1 introduced the persistent connections i.e. connections weren’t closed by default and were kept open which allowed multiple sequential requests. To close the connections, the header Connection: close had to be available on the request. Clients usually send this header in the last request to safely close the connection.
- Pipelining It also introduced the support for pipelining, where the client could send multiple requests to the server without waiting for the response from server on the same connection and server had to send the response in the same sequence in which requests were received. But how does the client know that this is the point where first response download completes and the content for next response starts, you may ask! Well, to solve this, there must be Content-Length header present which clients can use to identify where the response ends and it can start waiting for the next response.
- Chunked Transfers In case of dynamic content, when the server cannot really find out the Content-Length when the transmission starts, it may start sending the content in pieces (chunk by chunk) and add the Content-Length for each chunk when it is sent. And when all of the chunks are sent i.e. whole transmission has completed, it sends an empty chunk i.e. the one with Content-Length set to zero in order to identify the client that transmission has completed. In order to notify the client about the chunked transfer, server includes the header Transfer-Encoding: chunked
- Unlike HTTP/1.0 which had Basic authentication only, HTTP/1.1 included digest and proxy authentication
- Caching
- Byte Ranges
- Character sets
- Language negotiation
- Client cookies
- Enhanced compression support
- New status codes
- ..and more
The reason is, in HTTP/1.1 it can only have one outstanding connection at any moment of time. HTTP/1.1 tried to fix this by introducing pipelining but it didn’t completely address the issue because of the head-of-line blocking where a slow or heavy request may block the requests behind and once a request gets stuck in a pipeline, it will have to wait for the next requests to be fulfilled.
### SPDY - 2009
Google went ahead and started experimenting with alternative protocols to make the web faster and improving web security while reducing the latency of web pages.

The features of SPDY included, multiplexing, compression, prioritization, security etc.

SPDY didn’t really try to replace HTTP; it was a translation layer over HTTP which existed at the application layer and modified the request before sending it over to the wire. It started to become a defacto standards and majority of browsers started implementing it.

In 2015, at Google, they didn’t want to have two competing standards and so they decided to merge it into HTTP while giving birth to HTTP/2 and deprecating SPDY.
### HTTP/2 - 2015
The key features or differences from the old version of HTTP/1.1 include
- Binary instead of Textual
- Multiplexing - Multiple asynchronous HTTP requests over a single connection
- Header compression using HPACK
- Server Push - Multiple responses for single request
- Request Prioritization
- Security
![[Pasted image 20250622092849.png]]

## HTTP for Now
### HTTP Request
 A typical HTTP request contains:
1. HTTP version type
2. a URL
3. an HTTP method
	An HTTP method, sometimes referred to as an HTTP verb, indicates the action that the HTTP request expects from the queried server.
4. HTTP request headers
	![[Pasted image 20250622143747.png]]
5. Optional HTTP body.
	The body of a request is the part that contains the ‘body’ of information the request is transferring. The body of an HTTP request contains any information being submitted to the web server.
### HTTP response

An HTTP response is what web clients (often browsers) receive from an Internet server in answer to an HTTP request. These responses communicate valuable information based on what was asked for in the HTTP request.
A typical HTTP response contains:
1. an HTTP status code
	HTTP status codes are 3-digit codes most often used to indicate whether an HTTP request has been successfully completed. Status codes are broken into the following 5 blocks:
	1. 1xx Informational
	2. 2xx Success
	3. 3xx Redirection
	4. 4xx Client Error
	5. 5xx Server Error
	The “xx” refers to different numbers between 00 and 99.
2. HTTP response headers
	![[Pasted image 20250622144157.png]]
3. optional HTTP body