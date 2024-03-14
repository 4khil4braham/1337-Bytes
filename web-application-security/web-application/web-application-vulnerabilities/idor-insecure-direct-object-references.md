# 🟨 IDOR - Insecure Direct Object References

IDOR (Insecure Direct Object References) is a security vulnerability that occurs when an application exposes sensitive data or functionality by failing to properly validate user input related to object references, such as file paths, database keys, or URLs. Attackers can exploit IDOR vulnerabilities to access unauthorized resources, view sensitive information, or perform actions on behalf of other users.

Here's an explanation of IDOR with an example:

Suppose you have a web application that allows users to view their own profile information by providing their user ID in the URL:

```plaintext
http://example.com/profile?id=12345
```

In this example, the `id` parameter in the URL specifies the user ID. The server retrieves the corresponding user profile from the database and displays it to the user.

However, the application fails to validate the user ID or enforce authorization checks properly, allowing users to manipulate the `id` parameter in the URL to access other users' profiles.

For instance, if the attacker changes the `id` parameter to a different value (e.g., `id=54321`), the server will retrieve and display the profile information for the user with that ID, regardless of whether the attacker has permission to access it.

This means that the attacker can access sensitive information about other users, such as their email addresses, phone numbers, or even financial details, by simply changing the user ID in the URL.

An IDOR vulnerability can also extend to other types of objects, such as files or database records. For example, if a file download functionality in the application uses unvalidated file paths or database keys, an attacker could manipulate the input to download or access files or records they're not authorized to access.

To mitigate IDOR vulnerabilities, web developers should implement proper authorization checks and access controls to ensure that users can only access resources they are authorized to view or manipulate. This includes validating and enforcing access controls at both the application and server levels and implementing appropriate user authentication mechanisms to verify users' identities before granting access to sensitive resources. Additionally, developers should avoid exposing internal object references directly in URLs or other client-facing interfaces and instead use indirect references or identifiers that are securely mapped to corresponding objects on the server.
