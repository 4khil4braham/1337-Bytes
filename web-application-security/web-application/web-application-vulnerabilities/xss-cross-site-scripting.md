# 🟨 XSS - Cross-Site Scripting

Cross-site scripting (XSS) is a security vulnerability in web applications that allows attackers to inject malicious scripts into web pages viewed by other users. These scripts can then execute in the context of the victim's browser, allowing the attacker to steal sensitive information, hijack sessions, or perform other malicious actions.

Here's an example of how a reflected XSS attack might occur:

Suppose we have a vulnerable web application that displays user input without proper sanitization or validation:

```html
<!DOCTYPE html>
<html>
<head>
    <title>Search Results</title>
</head>
<body>
    <h1>Search Results</h1>
    <p>Your search query: <script>document.write(window.location.href)</script></p>
</body>
</html>
```

In this example, the web application displays the user's search query in the page content without properly sanitizing it. An attacker can exploit this vulnerability by crafting a malicious URL that includes a JavaScript payload:

```
http://vulnerable-site.com/search?q=<script>alert('XSS attack!')</script>
```

When a victim user visits the malicious URL, the browser makes a request to the vulnerable web application with the crafted search query. The web application then reflects this query in the page content without sanitization, resulting in the execution of the injected JavaScript payload:

```html
<!DOCTYPE html>
<html>
<head>
    <title>Search Results</title>
</head>
<body>
    <h1>Search Results</h1>
    <p>Your search query: <script>alert('XSS attack!')</script></p>
</body>
</html>
```

As a result, the victim sees an alert message saying "XSS attack!" pop up in their browser. This demonstrates how an attacker can exploit an XSS vulnerability to execute arbitrary JavaScript code in the context of the victim's browser.

To defend against XSS attacks, web developers should always sanitize and validate user input before displaying it on web pages. This includes escaping special characters and HTML entities to prevent scripts from being executed. Additionally, implementing Content Security Policy (CSP) headers and using secure coding practices can help mitigate the risk of XSS vulnerabilities in web applications.
