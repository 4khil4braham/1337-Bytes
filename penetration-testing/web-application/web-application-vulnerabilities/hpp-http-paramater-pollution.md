# 🟨 HPP - HTTP Paramater Pollution

HTTP parameter pollution (HPP) is a vulnerability that occurs when an attacker manipulates the parameters of a web request to modify the intended behavior of an application. This can lead to various security issues, such as bypassing access controls, injecting malicious code, or causing unexpected behavior.

Here's an explanation of HTTP parameter pollution with an example:

Suppose you have a web application that processes a search query submitted by a user. The application accepts multiple parameters in the URL query string to filter search results:

```
http://example.com/search?category=electronics&sort=price
```

In this example, the `category` parameter specifies the category of products to search for, and the `sort` parameter specifies the sorting criteria for the results.

Now, imagine an attacker manipulates the parameters of the URL to perform HTTP parameter pollution:

```
http://example.com/search?category=electronics&category=clothing&sort=price
```

In this manipulated URL, the attacker has duplicated the `category` parameter with different values (`electronics` and `clothing`). This could lead to unexpected behavior in the application, as the server might interpret the parameters differently depending on how it handles parameter parsing and duplicates.

For example, if the application processes the parameters using a simple approach like parsing the query string into a dictionary, it might only consider the last occurrence of a parameter and ignore the duplicates. In this case, the server would treat the search query as if it were for clothing products only, disregarding the electronics category specified earlier.

On the other hand, if the application concatenates or combines the parameter values into SQL queries or other data structures without proper validation or sanitization, it could result in SQL injection, cross-site scripting (XSS), or other security vulnerabilities.

Furthermore, if the application's logic relies on the presence or absence of certain parameters to determine access controls or perform actions, HTTP parameter pollution could potentially bypass these controls or cause unintended actions to be taken.

To mitigate HTTP parameter pollution vulnerabilities, developers should:

* Validate and sanitize all input parameters to ensure they contain expected values and do not contain malicious content.
* Implement proper parameter handling and parsing logic to handle duplicate or conflicting parameters appropriately.
* Use consistent naming conventions and avoid ambiguities in parameter names to reduce the risk of confusion and misinterpretation.
* Implement access controls and authorization checks based on the intended behavior of the application, rather than relying solely on the presence or absence of parameters.
* Regularly review and test the application's input handling and parameter parsing logic to identify and address potential vulnerabilities.
