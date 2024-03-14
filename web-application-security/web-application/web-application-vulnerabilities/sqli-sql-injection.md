# 🟨 SQLi - SQL Injection

SQL Injection is a type of security vulnerability that occurs when an attacker is able to manipulate SQL queries in a web application's input fields to execute arbitrary SQL code. This can lead to unauthorized access to the application's database, data leakage, data manipulation, and in some cases, complete compromise of the application.

Here's an example to illustrate SQL Injection:

Consider a simple web application that allows users to search for products in a database:

```python
# Python Flask example

from flask import Flask, request, render_template_string
import sqlite3

app = Flask(__name__)

@app.route('/')
def index():
    return """
    <form action="/search" method="post">
        Search: <input type="text" name="query">
        <input type="submit" value="Search">
    </form>
    """

@app.route('/search', methods=['POST'])
def search():
    query = request.form['query']
    conn = sqlite3.connect('products.db')
    cursor = conn.cursor()
    cursor.execute("SELECT * FROM products WHERE name LIKE '%" + query + "%'")
    results = cursor.fetchall()
    conn.close()
    return render_template_string("Search results: {{ results }}", results=results)

if __name__ == '__main__':
    app.run(debug=True)
```

In this example, the web application allows users to search for products by name in an SQLite database. However, the application is vulnerable to SQL Injection because it directly concatenates user input (the `query` parameter) into the SQL query without proper sanitization or parameterization.

An attacker can exploit this vulnerability by submitting a malicious input, such as `' OR 1=1--`, which would turn the SQL query into:

```sql
SELECT * FROM products WHERE name LIKE '%' OR 1=1-- '%'
```

In this injected query:

* The `OR 1=1` condition always evaluates to true, bypassing any filtering based on the original query.
* The `--` sequence comments out the rest of the SQL query, preventing syntax errors due to the injected code.

As a result, the SQL query returns all rows from the `products` table, effectively exposing all products in the database to the attacker.

To prevent SQL Injection, web developers should use parameterized queries or prepared statements instead of concatenating user input directly into SQL queries. Parameterized queries separate SQL code from user input, preventing attackers from injecting arbitrary SQL code into queries. Additionally, input validation and proper access controls can help mitigate the risk of SQL Injection vulnerabilities.
