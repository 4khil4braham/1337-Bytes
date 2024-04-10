# 🟧 JWT

## Overview

JWT (JSON Web Token) is a compact, self-contained way to securely transmit information between parties as a JSON object. It's commonly used for authentication and authorization in web applications and APIs. JWTs are composed of three parts: a header, a payload, and a signature.

## Working

1.  **Creating a JWT**:

    * **Header**: The header typically consists of two parts: the type of the token, which is JWT, and the signing algorithm being used, such as HMAC SHA256 or RSA. Example:

    ```json
    jsonCopy code{
      "alg": "HS256",
      "typ": "JWT"
    }
    ```

    * **Payload**: The payload contains the claims. Claims are statements about an entity (typically, the user) and additional data. There are three types of claims: registered, public, and private claims. Example:

    ```json
    jsonCopy code{
      "sub": "1234567890",
      "name": "John Doe",
      "admin": true
    }
    ```

    * **Signature**: The signature is created by encoding the header and payload using a specified algorithm and a secret key. This ensures that the token has not been tampered with. Example:

    ```scss
    scssCopy codeHMACSHA256(
      base64UrlEncode(header) + "." +
      base64UrlEncode(payload),
      secret
    )
    ```
2. **Sending the JWT**: Once created, the JWT can be sent to the client either as a response to a successful authentication request or as a token for accessing protected resources.
3. **Verifying the JWT**:
   * The client receives the JWT and includes it in the Authorization header of subsequent requests.
   * The server receives the JWT and verifies its authenticity and integrity by decoding the header and payload, re-creating the signature, and comparing it with the received signature.
4. **Accessing Claims**:
   * Once verified, the server can access the claims contained in the payload to determine the identity of the user and to make authorization decisions
