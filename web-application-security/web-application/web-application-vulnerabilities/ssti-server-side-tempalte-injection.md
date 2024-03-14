# 🟨 SSTI - Server Side Tempalte Injection

Server-side template Injection (SSTI) is a vulnerability that occurs when an attacker is able to inject malicious code into a server-side template. This allows the attacker to execute arbitrary code on the server and potentially gain unauthorized access or manipulate the application's behavior.

Here's an explanation of SSTI with an example using the Flask framework in Python:

Suppose you have a Flask web application that uses Jinja2 templates for rendering HTML pages:

```python
from flask import Flask, render_template, request

app = Flask(__name__)

@app.route('/')
def index():
    return render_template('index.html')

@app.route('/search', methods=['POST'])
def search():
    query = request.form['query']
    # Assume this is a vulnerable code where user input is directly used in template rendering
    return render_template('search_results.html', query=query)

if __name__ == '__main__':
    app.run(debug=True)
```

In this example, when the user submits a search query via a POST request to the `/search` endpoint, the server returns the search results page (`search_results.html`) and passes the user's input (`query`) to the template for rendering.

Now, let's consider a vulnerable template (`search_results.html`) that is susceptible to SSTI:

```html
<!DOCTYPE html>
<html>
<head>
    <title>Search Results</title>
</head>
<body>
    <h1>Search Results</h1>
    <p>Your search query: {{ query }}</p>
    <!-- This is the vulnerable part -->
    <p>Search results:</p>
    {{ 7 * 7 }}
</body>
</html>
```

In this vulnerable template, instead of displaying the actual search results, the server directly evaluates and renders the expression `7 * 7`. An attacker can exploit this vulnerability by injecting malicious code into the `query` parameter, leading to server-side code execution.

For example, if the attacker submits the following search query:

```
{{ ''.__class__.__mro__[1].__subclasses__() }}
```

The server will evaluate this expression and return a list of Python classes, providing the attacker with valuable information about the server's environment and potentially allowing them to execute arbitrary code.

To mitigate SSTI vulnerabilities, it's crucial to properly sanitize and validate user input before using it in template rendering. Additionally, using context-specific escaping functions and disabling certain template features can help prevent attackers from injecting and executing malicious code in server-side templates.
