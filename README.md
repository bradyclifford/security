# Overview

# Injection
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

# Cross Site Scripting (XSS)
* Output encoding is the cornerstone of XSS protection
* Code your application as though you have persistent XSS in the database
* Expect attackers to use obfuscated URLs

# Broken Authentication and Session Management
* Keep session Ids out of the URL, use cookie instead
  * Don't expect anything in the URL to be secure
* If possible, disable sliding forms authentication expiration

# Insecure Direct Object References
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
