# 🟨 HTTP Request Smuggling

HTTP request smuggling is a web security vulnerability that occurs when the front-end and back-end servers interpret the HTTP request headers differently, leading to discrepancies in how they process the request. This discrepancy can be exploited by attackers to perform various attacks, including request smuggling, cache poisoning, and session fixation.

Here's an explanation of HTTP request smuggling with an example:

Suppose you have a web application deployed behind a load balancer that forwards HTTP requests to backend servers. The load balancer and backend servers use different HTTP parsers or interpret HTTP headers differently, creating inconsistencies in how they handle certain request attributes, such as content length or transfer encoding.

Let's consider an example scenario involving a vulnerable web application and a load balancer:

1.  **Vulnerable Web Application**: The vulnerable web application processes HTTP requests with the following structure:

    ```
    POST /vulnerable HTTP/1.1
    Host: example.com
    Content-Length: 10

    0
    ```

    In this example, the `Content-Length` header specifies the length of the request body.
2.  **Load Balancer**: The load balancer interprets the request differently and splits it based on the `Content-Length` header:

    ```
    POST /vulnerable HTTP/1.1
    Host: example.com
    Content-Length: 10

    0
    ```

    In this case, the load balancer forwards the entire request, including the request body, to the backend server.
3.  **Backend Server**: The backend server interprets the request differently and processes it based on the `Transfer-Encoding` header:

    ```
    POST /vulnerable HTTP/1.1
    Host: example.com
    Transfer-Encoding: chunked

    0
    ```

    Here, the backend server treats the request as a chunked transfer encoding, where the request body is encoded in chunks.

Now, an attacker can exploit this inconsistency by crafting a malicious request that confuses the load balancer and backend server. For example, the attacker can send a request with a fake `Content-Length` header followed by a chunked request body:

```
POST /vulnerable HTTP/1.1
Host: example.com
Content-Length: 4
Transfer-Encoding: chunked

0

G
GET /admin HTTP/1.1
Host: example.com
```

In this example, the load balancer may interpret the request body's length as 4 bytes and forward the entire request to the backend server. However, the backend server may parse the request differently and interpret the request body as a chunked transfer encoding, treating the subsequent `GET /admin HTTP/1.1` request as a separate HTTP request.

As a result, the attacker can smuggle an additional HTTP request (`GET /admin HTTP/1.1`) to the backend server, bypassing any security controls or access restrictions enforced by the web application.

To mitigate HTTP request smuggling vulnerabilities, developers and system administrators should:

* Ensure consistent interpretation and handling of HTTP headers across all components of the web application infrastructure.
* Use standardized HTTP parsers and libraries that comply with the HTTP specifications to parse and process HTTP requests and responses.
* Implement strict input validation and sanitization to prevent maliciously crafted requests from bypassing security controls or causing unintended behavior.
* Regularly monitor and audit web application traffic to detect and mitigate potential security vulnerabilities, including HTTP request smuggling attacks.
