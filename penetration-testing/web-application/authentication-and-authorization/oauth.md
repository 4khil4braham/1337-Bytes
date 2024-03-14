# 🟧 OAuth

## Overview

OAuth (Open Authorization) is an open standard protocol that allows third-party applications to access resources (like user data) on a server, without needing the user's credentials. It's commonly used for enabling secure access to APIs and web services.

## Working

Imagine you're developing a mobile application that allows users to sign in with their Google account and access their Google Drive files.

1. **Client Registration**: Before using OAuth, you (the developer) need to register your application with Google. This involves providing information about your app, such as its name, website, and redirect URI.
2. **Authorization Request**: When a user tries to sign in to your app using their Google account, your app redirects them to Google's authorization server with a request for permission to access their Google Drive files.
3. **User Authentication**: The user is then prompted to log in to their Google account (if they haven't already) and grant your app permission to access their Drive files.
4. **Authorization Grant**: Once the user grants permission, Google's authorization server sends your app an authorization code.
5. **Access Token Request**: Your app then sends this authorization code, along with its client ID and secret, back to Google's authorization server, requesting an access token.
6. **Access Token Grant**: If everything checks out, Google's authorization server responds with an access token. This token is essentially a key that your app can use to access the user's Drive files on their behalf.
7. **Resource Access**: Now armed with the access token, your app can make requests to Google's Drive API, including commands to view, edit, or upload files.
8. **Token Expiry and Refresh**: Access tokens typically have a limited lifespan (e.g., one hour). When the token expires, your app can use a refresh token (if provided) to obtain a new access token without requiring the user to log in again.
9. **Handling Errors and Revocations**: OAuth also provides mechanisms for handling errors, such as if the user denies permission or if the access token is invalid. Additionally, users can revoke access to their data at any time through their Google account settings.

Overall, OAuth facilitates secure, delegated access to resources by allowing users to grant limited permissions to third-party applications without sharing their credentials. It's widely used across the web for enabling features like social login, API integration, and single sign-on.
