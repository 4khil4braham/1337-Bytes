# 🟧 OAuth

## Overview

OAuth (Open Authorization) is an open standard protocol that allows third-party applications to access resources (like user data) on a server, without needing the user's credentials. It's commonly used for enabling secure access to APIs and web services.

## Working

1. **Client Registration**: The third-party application (client) registers with the service provider (also known as the OAuth provider) and obtains a client ID and client secret. These are used to authenticate the client to the server.
2. **Authorization Request**: When a user tries to access a resource on the server using the client application, the application redirects the user to the server's authorization endpoint along with its client ID and requested permissions (scopes).
3. **User Authentication**: The server authenticates the user's identity. If the user isn't logged in, they're prompted to log in to the server.
4. **Authorization Grant**: After authentication, the server presents the user with a consent screen, showing the requested permissions that the client application is asking for. The user grants or denies permission to the client.
5. **Authorization Grant Issuance**: If the user grants permission, the server issues an authorization grant to the client. This grant can take various forms depending on the OAuth flow being used (e.g., authorization code, implicit, client credentials, etc.).
6. **Access Token Request**: The client application exchanges the authorization grant for an access token by making a request to the server's token endpoint, including the authorization grant and its client credentials.
7. **Access Token Issuance**: The server verifies the authorization grant and client credentials and issues an access token to the client application. This access token represents the client's permission to access the user's resources.
8. **Resource Access**: The client application includes the access token in requests to the server's resource endpoints. The server validates the access token and grants or denies access to the requested resources based on the permissions associated with the token.
9. **Token Expiry and Refresh**: Access tokens usually have a limited lifespan. When they expire, the client can use a refresh token (if provided) to obtain a new access token without needing the user to re-authenticate.
