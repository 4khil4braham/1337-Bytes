# 🟨 LFI - Local File Inclusion

Local File Inclusion (LFI) is a type of vulnerability that occurs when a web application includes a local file (usually on the server's filesystem) in its output without proper sanitization or validation. Attackers can exploit LFI vulnerabilities to read sensitive files, execute arbitrary code, or achieve remote code execution.

Here's an explanation of LFI with an example:

Suppose you have a vulnerable PHP web application that includes a file based on a user-supplied parameter:

```php
<?php
// vulnerable.php

$file = $_GET['file']; // User-supplied input

include($file); // Include the specified file
?>
```

In this example, the PHP script includes a file specified by the `file` parameter from the URL query string. However, it does not properly validate or sanitize the input, making it vulnerable to LFI.

An attacker can exploit this vulnerability by supplying a malicious file path in the `file` parameter, allowing them to read arbitrary files from the server's filesystem.

For example, if the attacker submits the following URL:

```
http://vulnerable-site.com/vulnerable.php?file=/etc/passwd
```

The vulnerable PHP script will attempt to include the `/etc/passwd` file from the server's filesystem. If the server has read permissions for the `/etc/passwd` file, its contents will be included in the output and returned to the attacker, potentially revealing sensitive information such as user account details.

Additionally, attackers can leverage LFI vulnerabilities to execute arbitrary code by including PHP scripts from the server's filesystem. For example, if the attacker submits the following URL:

```
http://vulnerable-site.com/vulnerable.php?file=../../../../../var/www/malicious.php
```

And if the server has PHP code execution enabled, the `malicious.php` script will be executed, allowing the attacker to run arbitrary commands or code on the server.

To mitigate LFI vulnerabilities, web developers should:

* Avoid including files based on user-supplied input.
* If including files is necessary, validate and sanitize the input to ensure it refers only to allowed files.
* Limit access permissions to sensitive files and directories to prevent unauthorized access.
* Implement proper input validation and output encoding to prevent other types of injection attacks.
