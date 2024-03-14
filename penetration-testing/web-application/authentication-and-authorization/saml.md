# 🟧 SAML

## Overview

SAML stands for Security Assertion Markup Language. It is an XML-based open standard for exchanging authentication and authorization data between an identity provider (IdP) and a service provider (SP). Essentially, SAML enables secure web domains to exchange user authentication and authorization data, simplifying the process of logging on to multiple applications or websites without needing to create new credentials for each one. Let's break down how SAML works and its key components:

## Working of SAML

1. **User Access Request**: The process begins when a user attempts to access a resource or service provided by the Service Provider (SP).
2. **SP Initiated SSO (Single Sign-On) or Authentication Request**: The SP generates a SAML Authentication Request and redirects the user's browser to the Identity Provider (IdP). This request typically includes metadata about the SP and may specify the requested authentication context.
3. **User Authentication**: The IdP receives the authentication request and authenticates the user. This authentication can involve various methods such as username/password, multi-factor authentication, or other mechanisms depending on IdP configurations.
4. **Assertion Generation**: After successful authentication, the IdP creates a SAML assertion. This assertion contains information about the user (such as their identity, attributes, and possibly entitlements), metadata about the assertion (e.g., expiration time, issuer), and may be digitally signed by the IdP to ensure its integrity.
5. **SAML Response**: The IdP sends the SAML assertion back to the user's browser, typically as part of an HTML form. This form is automatically submitted to the SP via the user's browser.
6. **Assertion Consumption**: The SP receives the SAML response. It verifies the digital signature on the assertion to ensure its authenticity and integrity. It also checks the assertion's metadata to ensure it hasn't expired and that it was issued by a trusted IdP.
7. **Session Establishment**: If the assertion is valid and trusted, the SP establishes a session for the user and grants access to the requested resource or service. The user is considered authenticated within the SP's domain.
8. **Authorization and Resource Access**: Once authenticated, the user can access the requested resource or service provided by the SP, based on the permissions and authorization rules configured within the SP's system.
