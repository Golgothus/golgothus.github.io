# Web Fundamentals

Typical ports for web servers would be:

- 80 (http, non-encrypted, unsecure)
- 443 (ssl / tls, encrypted / secure)

Popular Web Server examples:

- Apache
- Nginx
- Microsoft's IIS

Web page content can consist of any of the following resources:

- HTML (hyper text mark-up language)
- CSS (Cascading style sheet)
- Javascript / JSON (scripting language to handled client / backend events)
- PHP

List of HTTP(S) Responses:

- 100-199: Information
- 200-299: Successes (200 OK is the "normal" response for a GET)
- 300-399: Redirects (the information you want is elsewhere)
- 400-499: Client errors (You did something wrong, like asking for something that doesn't exist)
- 500-599: Server errors (The server tried, but something went wrong on their side)

Further reading [Mozilla](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status)

>For POST requests, this is the content that's sent to the server. For GET requests, a body is allowed but will mostly be ignored by the server. The response will also have a body. For GET requests, this is normally web content or information such as JSON. For POST requests, it may be a status message or similar.

[Further reading on cookies](https://developer.mozilla.org/en-US/docs/Web/HTTP/Cookies)

## Task 5 - Mini CTF

- **GET request.** Make a GET request to the web server with path /ctf/get
- **POST request.** Make a POST request with the body "flag_please" to /ctf/post
- **Get a cookie.** Make a GET request to /ctf/getcookie and check the cookie the server gives you
- **Set a cookie.** Set a cookie with name "flagpls" and value "flagpls" in your devtools and make a GET request to /ctf/sendcookie

### Task 5 - Flag1 - What's the get flag

For this, we are going to use curl. Deploy the machine, and use curl against whatever the address / system you are provided.

```bash
curl <ipaddress:port>/ctf/get
```

This should provide you the flag, as curl by default is essentially a GET request

### Task 5 - Flag2 - What's the POST flag

From reading the information provided on TryHackMe we know that we can us the parameter of -X \<type of request> to try and get different HTTP responses

For example:

```bash
curl -X POST <website>
```

The only thing is, when using POST is that we need to provide the information that we are POSTing to the site

```bash
curl -X POST -d <POST request / body> <website>
```

We find out that using the flag -d (data) will allow us to enter a body / request information for POST

So we can essentially do:

```bash
curl -X POST -d "flag_please" <website>
```

### Task 5 - flag3 What is the get cookie flag

It's best to consult the man pages again for curl

After reading through it about 5 times I saw that -c takes a parameter for \<filename>, and mentions that **"Specify to which file you want curl to write all cookies after a completed operation."**

```bash
curl -c <filename to write cookie to> <website> && cat <filename>
```

### Task 5 - Flag4 What is the send cookie flag

Again, RTFM, read the **friendly** manual. It will help, and only provide assistance along the way

We see the syntax is similar to below for "send-cookie"

curl -b "cookiename:cookievalue" \<website>

We can also use -c \<filename> to write the locally stored cookies out in conjuction with the -b switch

```bash
curl -b "flagpls=flagpls;" -c <filename> http://10.10.171.68:8081/ctf/sendcookie && cat <filename>
```
