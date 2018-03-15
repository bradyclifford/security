# Overview

# OWASP Top 10

## 1. Injection
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

## 2. Cross Site Scripting (XSS)
* Output encoding is the cornerstone of XSS protection
* Code your application as though you have persistent XSS in the database
* Expect attackers to use obfuscated URLs

## 3. Broken Authentication and Session Management
* Keep session Ids out of the URL, use cookie instead
  * Don't expect anything in the URL to be secure
* If possible, disable sliding forms authentication expiration

## 4. Insecure Direct Object References
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

## 5. Security Misconfiguration
* Don't expose descriptive errors to the client in production
* Keep frameworks current
* Protect sensitive data in the configuration files
* Automate security configuration using transforms

## 6. Insecure Cryptographic Storage
* Cryptographic storage is the last line of defence
* Password hashing is all about trying to slow the process down in order to increase the time and cost of cracking
  * Creating higher workloads with approaches like `PBKDF2` and `brcypt` is the best defence
* Key management is importat; `DPAPI` makes it easy to solve but inroduces othe problems (multiple machines, local key)
* Character rotation and encoding aren't cryptographic!

### Tools
* `Hastcat` used to compute hashes
* `Zetetic.Security` uses 5,000 computations of `PBKDF2`, and 2^10 rounds of `bcrypt`. Takes 50 days to crack.

### Terms
* Hasing: one way, not encryption
* Symmetric encryption: single private key for both encryption and decryption
* Asymmetric encryption: both a public and private key

## 7. Failure to Restrict URL Access
...
## 8. Insufficent Transport Layer Protection
* Cookies can be sensitive due to what they allow the attacker to do; flag them as secure
* Make sure that resources which require secure connections cannot be loaded over HTTP
Use HSTS for secure sites
* Avoid security anti-patterns
  * Loading login forms over HTTP, even if they post to HTTPS
  * Loading HTTPS login forms inside an iframe on an HTTP page
  * Login page should never be allowed to load over HTTP
  * Passing sensitive data such as credentials in HTTP addresses
* SSL has some overhead, just not much and its also not fool proof

*Protocal URL*: instead of defining the protocal, `http://somedomain.com`, substitute with `//somedomain.com/folder/`.  It will automatically add the protocal prefix being used.

`HSTS` possibly can add a header to restrict HTTP requests.
```
Strict-Transport-Security max-age=3153600 
// for the next seconds (12 months), 
// browser may not make an HTTP request to the site
```

Anti TLS patterns

