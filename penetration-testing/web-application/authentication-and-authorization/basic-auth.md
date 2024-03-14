# 🟧 Basic Auth

## Overview

Basic Authentication is a simple authentication mechanism commonly used in HTTP-based applications, where the client sends a username and password encoded in Base64 format with each request. While simple, it's not the most secure method of authentication since the credentials are sent as plaintext and can be intercepted.

## Working

1. **Client Request**: The client (for example, a web browser or a mobile app) attempts to access a protected resource on a server that requires authentication.
2. **Server Response**: The server responds with a 401 Unauthorized HTTP status code, indicating that authentication is required to access the resource.
3. **Client Credential Submission**: The client prompts the user to enter their username and password.
4. **Credential Encoding**: The client combines the username and password into a string in the format "username:password" (e.g., "john\_doe:secretpassword") and encodes this string using Base64 encoding.
5. **Authorization Header**: The client includes an Authorization header in the request with the value "Basic" followed by the Base64-encoded credentials. The header looks like this:

```http
Authorization: Basic am9obi5kb2U6c2VjcmV0cGFzc3dvcmQ=
```

6. **Server Authentication**: The server extracts the Base64-encoded credentials from the Authorization header and decodes them. It then verifies the credentials against its authentication system (e.g., a user database).
7. **Access Granted or Denied**: If the credentials are valid, the server grants access to the requested resource and responds with the content. Otherwise, it returns another 401 Unauthorized status code or a different error status code, indicating access denial.
