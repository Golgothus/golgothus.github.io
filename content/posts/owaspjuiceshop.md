# THM Beginner Path - OWASP JuiceShop

> Admin email - admin@juice-sh.op

## Task 4 - Who broke my lock

### Task 4 - Question #1: Bruteforce the Administrator account's password

Review over [Broken Authentication](https://owasp.org/www-project-top-ten/OWASP_Top_Ten_2017/Top_10-2017_A2-Broken_Authentication)

- We are reviewing weak passwords in high priv accounts
- Forgotten password pages

For this section we are using a password list from the best1050 used passwords, this can be obtained by running:

> apt-get install seclists

You can load the list from:

```bash
/usr/share/seclists/Passwords/Common-Credentials/best1050.txt
```

[Burp Intruder > Positions Payload Marker](https://imgur.com/Qjv4Apt.png)

[Burp Intruder > Payloads](https://imgur.com/Ab7wFsr.png)

### Task 4 - Question #2: Reset Jim's password

Here we are essentially going to do some good ol' OSINT on our friend Jim

> jim@juice-sh.op

When we go to our **forgot password** page, we notice that it provides the information on the user's security questions. For Jim, the security question is "what is the middle name of your eldest sibling".

From one of our previous tasks we find out that Jim enjoys the show Star Trek. So now time for some Googling, Binging, Duck Hunting, whatever suits your fancy.

## Task 5 - AH, Don't Look

### Task 5 - Question #1: Access the Confidential Document

For this one, direct link associated with the file is of some interests

We view the About Us page, hover over the "terms of use" link, and see the URL goes to an openly available FTP (File Transfer Protocol) server

Here we see a few folders / directories which are now *ours* for the taking

### Task 5 - Question #2: Log into MC SafeSearch's account

Now we are going to do some recon on MC SafeSearch, and it seems that we are provided information by the peeps over at TryHackMe who have kindly linked us the CollegeHumor video associated with this account:

> mc.safesearch@juice-sh.op

In the video MC Safe Search mentions the format of his password, so now we log ourselves right on in!

[MC Safe Search!](https://youtu.be/v59CX2DiX0Y)

### Task 5 - Question #3: Download the Backup file

So our goal is to download the .bak file from the FTP directory. However, we are now getting a 403 due to the inability of downloading this certain filetype / extension.

We will use what's called a **Poison Null Byte**

> Poison null byte looks like this **%00**. Note that we can download it using the url, so we will encode this into a url encoded format.
>
> I used [CyberChef](https://gchq.github.io/CyberChef/) to get the URL-Encoded value
>
> Giving us **%0025**

We then append the URL encoded poisoned null byte to the end of our URL to gain access to the .bak file

## Task 6 Who's flying this thing

In this section we are reviewing broken access control. Essentially we are going to try and pivot from a normal user into someone who has privileged permissions. There are two known types of access control, vertical and horizontal.

|Horizontal Privilege Escalation | Occurs when a user can perform an action or access data of another user with the same level of permissions.|
|---|---|
| Vertical Privilege Escalation | Occurs when a user can perform an action or access data of another user with a higher level of permissions.|

For more info on broken access controls we can view the official [OWASP site](https://owasp.org/www-project-top-ten/OWASP_Top_Ten_2017/Top_10-2017_A5-Broken_Access_Control)

### Task 6 - Question #1: Access the administration page

The TryHackMe site tells us to go through and look at a specific Javascript file for "admin".

They look for "app-administration", however this is not the actual value / URL we are wanting to go to.

I ended up searching a little while longer and found the following information:

```javascript
Xs=[{path:"administration"
```

This is the actual site we will be looking to get access to. So we do the following:

1. Login to the admin user *admin@juice-sh.op*
2. Go to [box IP]/#/administration
3. Boomski's you should now have you flag

### Task 6 - Question #2: View another user's shopping basket

Check this out now Super Shopper's we are swapping our carts out for someone else's sweet basket of Raspberry Juice!

Essentially we go through and do the following:

- Make sure that you are currently intercepting traffic with either Burp or Zap
- Top right, click on Basket
- Go through and forward your intercepts until you get the following URL

```html
GET /rest/basket/1 HTTP/1.1
```

- Similar to [this image](https://imgur.com/i5oVY5O.png)

- We can manipulate the number following basket to be another "userid" similar to [this image](https://imgur.com/LJd5a63.png) as an example the intercepted request for userid 2 would look like:

```html
GET /rest/basket/2 HTTP/1.1
```

### Task 6 - Question #3: Remove all 5-star reviews

Go back to the [box ip]/#/administration/ page, and remove the only 5-star review

## [Task 7] Where did that come from

There are three major types of XSS attacks:

```text
DOM (Special)
Persistent (Server-side)
Reflected (Client-side)
```

**DOM XSS** (Document Object Model-based Cross-site Scripting) uses the HTML environment to execute malicious javascript. This type of attack commonly uses the \<script></script> HTML tag.

**Persistent XSS** is javascript that is run when the server loads the page containing it. These can occur when the server does not sanitise the user data when it is uploaded to a page. These are commonly found on blog posts.

**Reflected XSS** is javascript that is run on the client-side end of the web application. These are most commonly found when the server doesn't sanitise search data.

More information: [Cross-Site Scripting XSS](https://owasp.org/www-project-top-ten/OWASP_Top_Ten_2017/Top_10-2017_A7-Cross-Site_Scripting_(XSS))

### Task 7 - Question #1: Perform a DOM XSS

We are going to perform DOM level XSS.

- Top right click the search glass
- type in:

```html
<iframe src="javascript:alert(`xss`)">
```

- Press enter, this will give you teh answer

There are several other "methods" for DOM XSS as well, however this specific one did not work for this flag:

```javascript
<script>alert('foo')</script>
```

You can also attempt to run javascript as URL encoded XSS

```javascript
%22%3e%3cscript%3ealert(%22foo%22)%3c/script%3e
```

### Task 7 - Question #2: Perform a persistent XSS

From here we are going to store XSS into the IP Login section of the user profile for admin

- Top right, go to Account > Privacy and Security > Last Login IP
- We see 0.0.0.0 as the "last login" ip
- Turn your intercept on in Burp / Zap
- Top right, go to Account > logout
- Go to your intercept > headers
- Add the new header for:

```html
True-Client-IP <iframe src="javascript:alert(`xss`)">
```

- Forward the request
- Sign back into admin
- Go to the last IP login
- See the XSS alert occur

### Task 7 - Question #3: Perform a reflected XSS

Login into the admin account and navigate to the 'Order History' page.

We will use the iframe XSS,

```html
<iframe src="javascript:alert(`xss`)">
```

in the place of the order id, for exaple *5267-f73dcd000abcc353*

Start by:

- Clicking the truck
- Cliking "track the order"
- Where you see an image tracking your order, we are going to manipulate the URL as the following:
10.10.55.113/#/track-result?id=5267-0252e4000e720f70

I ended up just doing a URL-encoded format for the XSS:

```html
http://10.10.55.113/#/track-result?id=%3Ciframe%20src%3D%22javascript:alert(%60xss%60)%22%3E
```

Paste this URL into the bar, and refresh the page and your XSS will appear
