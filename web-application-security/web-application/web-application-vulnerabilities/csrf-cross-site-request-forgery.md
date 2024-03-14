# 🟨 CSRF - Cross-Site Request Forgery

## Working

Suppose you have an online banking application where users can transfer money between accounts. When a user is logged in, the application displays a form to initiate a money transfer:

```html
htmlCopy code<form action="/transfer" method="post">
    <input type="hidden" name="from_account" value="123456">
    <input type="hidden" name="to_account" value="789012">
    <input type="hidden" name="amount" value="100">
    <input type="submit" value="Transfer Money">
</form>
```

When the user submits this form, it sends a POST request to the `/transfer` endpoint with parameters indicating the source account, destination account, and transfer amount.

Now, imagine an attacker creates a malicious website that contains the following HTML:

```html
htmlCopy code<html>
<head>
    <title>Malicious Website</title>
</head>
<body>
    <h1>Click the button!</h1>
    <img src="https://bankingapp.com/transfer?from_account=attacker&to_account=victim&amount=100" width="0" height="0">
    <script>
        document.write('<img src="https://bankingapp.com/transfer?from_account=attacker&to_account=victim&amount=100" width="0" height="0">');
    </script>
</body>
</html>
```

When a victim user visits this malicious website while logged in to their online banking application, their browser automatically sends requests to initiate a money transfer from their account to the attacker's account:

1. The `<img>` tag and JavaScript code in the malicious website both send requests to the `/transfer` endpoint with parameters specifying the source account as the victim's account, the destination account as the attacker's account, and the transfer amount.
2. Since the victim is authenticated to the banking application, their browser includes the necessary session cookies with the request, making it appear as if the victim initiated the transfer.
3. The banking application processes the request as if it came from the victim and transfers money from their account to the attacker's account.
4. The victim is unaware of this activity since they never intentionally initiated the transfer. They may only realize what happened after checking their account balance later.

## Mitigation

1. **CSRF Tokens**: Generate and include unique tokens in forms and requests that perform sensitive actions. These tokens are tied to the user's session and are verified by the server to ensure that the request originated from a legitimate form submission within the application.
2. **Same-Site Cookies**: Set the SameSite attribute on cookies to restrict usage to same-site requests. This helps mitigate CSRF attacks by preventing browsers from sending cookies on cross-site requests.
3. **Referrer Policy**: Implement a strict referrer policy that limits the information sent in the Referer header on outgoing requests. This helps prevent the leakage of sensitive information to external websites.
4. **Custom Headers**: Include custom headers in requests the server can validate to ensure they originated from a trusted source.
5. **Multi-Factor Authentication (MFA)**: Require users to authenticate using multiple factors (e.g., password and SMS code) before performing sensitive actions.
