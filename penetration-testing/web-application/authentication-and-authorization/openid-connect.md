# 🟧 OpenID Connect

## Overview

OpenID Connect (OIDC) is an identity layer built on top of OAuth 2.0, providing authentication services for web and mobile applications. It allows clients to verify the identity of end-users based on the authentication performed by an authorization server, as well as to obtain basic profile information about the user in an interoperable and REST-like manner.

## Working

1. **Client Registration**: A web application (the client) wants to allow users to sign in using their Google accounts. The client registers with Google's OpenID Connect provider by providing information such as its client ID and redirect URIs.
2. **Authorization Request**: When a user attempts to sign in to the web application, they are redirected to Google's authorization server. The client includes specific OpenID Connect parameters in the authorization request, indicating that it wants to authenticate the user.
3. **User Authentication**: The user is prompted to log in to their Google account (if they haven't already). Google's authorization server handles the authentication process securely.
4. **Authorization Grant**: Upon successful authentication, the authorization server issues an authorization code to the client application.
5. **Token Request**: The client application exchanges the authorization code for an ID token and an access token by sending a token request to Google's token endpoint.
6. **ID Token Verification**: The client verifies the ID token's signature to ensure its authenticity. It also checks that the token's issuer matches the expected value (Google) and that the token hasn't expired.
7. **User Information Request**: Optionally, the client can use the access token to request additional information about the user by sending a user info request to Google's userinfo endpoint. This might include details such as the user's name, email address, and profile picture.
8. **User Session Establishment**: The client establishes a session for the user, allowing them to access protected resources or perform actions within the application without needing to reauthenticate for subsequent requests.
9. **Token Expiry and Refresh**: Access tokens typically have a limited lifespan. When the token expires, the client can use a refresh token (if provided) to obtain a new access token without requiring the user to log in again.
10. **Single Sign-Out (Optional)**: OpenID Connect also supports single sign-out, allowing users to log out from all applications that use the same identity provider in one go.
