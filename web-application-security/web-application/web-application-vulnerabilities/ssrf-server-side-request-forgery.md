# 🟨 SSRF - Server-Side Request Forgery

Server-side request Forgery (SSRF) is a vulnerability that occurs when an attacker manipulates the server into making unintended requests to internal or external resources. This can lead to unauthorized access to internal systems, data leakage, or remote code execution.

Here's an explanation of SSRF with an example:

Suppose you have a web application that allows users to provide a URL and fetch the contents of that URL:

```python
from flask import Flask, request
import requests

app = Flask(__name__)

@app.route('/fetch')
def fetch_url():
    url = request.args.get('url')
    response = requests.get(url)
    return response.text

if __name__ == '__main__':
    app.run(debug=True)
```

In this example, the web application takes a URL from the user and uses the `requests.get()` function to fetch the contents of that URL. However, the application does not properly validate or sanitize the user-provided URL.

Now, consider a scenario where an attacker exploits this SSRF vulnerability to access internal resources or sensitive information. For example, the attacker could provide the URL of an internal system that is not intended to be exposed to the Internet:

```
http://example.com/fetch?url=http://internal-system.example.com/secret
```

In this case, the vulnerable web application will make a request to `http://internal-system.example.com/secret`, which is an internal resource not meant to be accessed from outside the network. If the internal system is vulnerable to certain attacks (such as default credentials, insecure configurations, or known vulnerabilities), the attacker could exploit those vulnerabilities to gain unauthorized access to sensitive data or even compromise the entire internal network.

Additionally, an attacker could exploit SSRF to perform attacks such as port scanning, service enumeration, or reconnaissance on internal systems. For example, the attacker could provide URLs with various IP addresses and port numbers to scan for open ports or vulnerable services within the internal network.

To mitigate SSRF vulnerabilities, developers should:

* Validate and sanitize all user-provided URLs to ensure they point to allowed resources.
* Use allowlists or domain restrictions to limit the domains or IP addresses that the application can access.
* Avoid making requests to URLs provided by users unless absolutely necessary, and if so, use techniques such as URL whitelisting or validation against a trusted list of domains.
* Run the web application in a restricted environment with limited network access or firewall rules to prevent access to sensitive internal resources.
* Monitor and log outgoing requests from the web application to detect and investigate potential SSRF attacks.
