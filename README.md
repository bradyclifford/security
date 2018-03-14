# Overview

# OWASP Top 10

## Injection
* Apply the principle of least privilege to the database account
* Always parameterise untrusted data
* Stored procedures also offer protection via parameterisation
  * You can still build an injection risk into them
  ```
  DECLARE @SearchTerm VARCHAR(50);
  SET @query = 'SELECT * FROM dbo.Product WHERE Name LIKE ''%'' + @SearchTerm + ''%'''
  EXEC sp+executesql @query, N'@SearchTerm varchar(50)', @SearchTerm=@SearchTerm
  ```
* Alway validate untrusted data against a whitelist
* Use ORMs and their native ability to parameterise
* Remeber the ease of exploitation by automated tools

## Cross Site Scripting (XSS)
* Output encoding is the cornerstone of XSS protection
* Code your application as though you have persistent XSS in the database
* Expect attackers to use obfuscated URLs

## Broken Authentication and Session Management
* Keep session Ids out of the URL, use cookie instead
  * Don't expect anything in the URL to be secure
* If possible, disable sliding forms authentication expiration

## Insecure Direct Object References
Use:
```
https://site.com/user?id=fasdfasdfasdfasdf23rq3regwtq34254365q25r13r
```
Instead of:
```
https://site.com/user?id=1
```
* Access controls are most fundemental. Does the authenticated user have authorization to that resource?
* Indirect references can be used to concel internal keys
* Surrogate key only adds further obfuscation; a `GUID` is a good example
  * GUID is slower than an integar and they take up more space

#3 Cross Site Requiest Forgery (CSRF)
Uses the cookie authentication for the domain site and sends a request to the same domain site.  A legitimate request is reconstructed into one with malicious intent.

The authenticated state of the victim is abused and the browser tricked into issuing the request.

* Use Anti-forgery tokens

## Security Misconfiguration
* Don't expose descriptive errors to the client in production
* Keep frameworks current
* Protect sensitive data in the configuration files
* Automate security configuration using transforms

## Insecure Cryptographic Storage
### Tools
* `Hastcat` used to compute hashes
* `Zetetic.Security` uses 5,000 computations of PBKDF2, and 2^10 rounds of bcrype. Takes 50 days to crack.
