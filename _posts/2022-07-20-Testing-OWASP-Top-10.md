---
layout: post
title:  "Testing OWASP Top 10"
date: 2022-07-19 10:55
category: general
---
To get a basic understanding of OWASP TOP 10, we are going to test the owasp top 10 vulnerabilites on a Vulnerable website/tool called as "JUICESHOP".

We will be covering the OWASP TOP 10, in the order of 2017 rankings.

{% highlight ruby %}Resources for OWASP Guide:{% endhighlight %}

OWASP Top 10: [https://owasp.org/Top10/](https://owasp.org/Top10/)

OWASP Testing Checklist: [https://github.com/tanprathan/OWASP-Testing-Checklist](https://github.com/tanprathan/OWASP-Testing-Checklist)

OWASP Testing Guide: [https://owasp.org/www-project-web-security-testing-guide/assets/archive/OWASP_Testing_Guide_v4.pdf](https://owasp.org/www-project-web-security-testing-guide/assets/archive/OWASP_Testing_Guide_v4.pdf)
   
   
Always download owasp testing checklist, to check across your tests, and it also gives almost 100+ tests to do on a website. 

{%highlight ruby%}Installing JUICESHOP:{%endhighlight%}

What is juiceshop ?

juiceshop is made by OWASP. It is a vulnerable website for us to learn a bunch of different OWASP top 10 attacks.

**Install docker in kali: https://github.com/juice-shop/juice-shop**

**Step1**: (curl -fsSL https://download.docker.com/linux/debian/gpg | gpg --dearmor | sudo tee /usr/share/keyrings/docker-archive-keyring.gpg >/dev/null)

**Step 2**: (echo 'deb  https://download.docker.com/linux/debian buster stable' | sudo tee /etc/apt/sources.list.d/docker.list)

**Step 3**: apt-get update

**Step 4**: sudo apt-get install -y docker-ce

**We then have to install juiceshop on docker: https://github.com/bkimminich/juice-shop**

**Step 1**: docker pull bkimminich/juice-shop

**Step 2**: docker run --rm -p 3000:3000 bkimminich/juice-shop

juiceshop gitbook for reference: https://pwning.owasp-juice.shop/

The gitbook is an amazing resource for pentesting a website and practicing it on juiceshop, it gives you a list of challenges that you can perform on juiceshop.

We are gonna do the part-II challenge hunting part, and try to solve all the challenges, ranging from 1-star to 7-star challenges, which is like the hardest to do.

We'll cover only those applicable to the owasp top 10, the rest you can practice on your own.

In Juiceshop, you can navigate to localhost:3000/#/score-board - to look at different types of vulnerabilities, and how many have you solved.

{%highlight ruby%}Installing FOXYPROXY:{%endhighlight%}

Its an easy way of switching the proxy ON and OFF when you need to capture the intermediate traffic.

Go into google, and type in foxy proxy, and add it to your plugins

Go into your FOXYPROXY options, give it a title, selecting HTTP is fine, and give your 127.0.0.1 address with port as 8080.

and thats it.

{%highlight ruby%}Exploring BURPSUITE:{%endhighlight%}

What to expect while doing a web-app pentest.

typically there are three stages of testing with web-apps:

1. You have an **unauthenticated stage**: i.e without creating an account on the website, and trying to navigate around the website looking for loopholes to exploit.
2. The **user tier**: i.e we login as a user, and we dont have admin priv, but we do have some user accessiblity, so we will have some extra access that we wouldnt have on the unauthenticated side.
3. As a **Admin user**: i.e giving us access to admin panel etc. i.e to see if you can access the admin panel from the unathenticated or user side etc or you can just bypass certain restrictions which should be there to get to the admin panel.

i.e it is always good to test who can get to where. We usually start with the unauthenticated side and just browse through to see what all is there in the website.

when you browse through website, go to burpsuite and under **target -> site** map, add the website **to scope**, and **filter the view to scope only.**

in the BS, you can also the **scan option**, but that is only available for the BS Pro Version, the scan feature will usually do **crawling** the website, which was used to be called as **spidering** in the BS Older versions.

crawling basically is trying to go out to different websites within the branches it has in the sitemap, and its gonna look into them and see where it can get access to.

But, anyways the scanning option is not that reliable, as its only known to pick up 10-15% of the vulnerabilities, so we should always rely on our manual scanning abilities.

while they are good to have, but they arent reliable.

**Features of BS**:

1. The proxy feature, to intercept the traffic. Giving us the option to modify the request or atleast see the traffic in transit.
2. we can send the request captured to repeater. Repeater is a request repeater, you can send the request from repeater and you can see the response that comes back.
3. In BS, you can go to Options under proxy, and checkmark **AND URL in target scope** under **intercept client request, and same for under intercept server request** - this will capture traffic only which is in the scope, and not the other types of traffic that comes and goes through in between.
4. We can also use intruder attacks in BS. Its actually a little slow for the community version - under the **extender->BApp Store**, we can download a Turbo Intruder, to increase the speed of our intruder attacks
5. There are other features like, comparer, decoder etc, but we are mostly going to use the proxy feature.

{%highlight ruby%}1. SQL Injection:{%endhighlight%}

SQLi is an attack in which malicious SQL Statements are injected into a SQL DB.

SQLi is easy to avoid, but still happens most often. If Successful, we can read sensitive DBs, extract info, modify DBs, and potentially even get a shell.

**Common SQL Verbs:**
**SELECT**: Retrieves data from a table.
**INSERT**: Adds data to a table.
**DELETE**: Removes data from a table.
**UPDATE**: Modifies data in a table.
**DROP**: Delete a table.
**UNION**: Combines data from multiple queries.
**WHERE**: Filters Records based on specific conditions.
**AND/OR/NOT**: Filter records based on multiple conditions.
**ORDER BY**: Sorts records in ascending/descending order.

Example:

![SQLiExample](https://raw.githubusercontent.com/Byteman0xD/Byteman0xD/main/assets/SQLiexample.png)

Special Characters in SQL:

' and " - string delimiters.
--, /*, and # - comment delimiters.
* and % - wildcards.
; - ends SQL statement.
Plus a bunch of others that follow programmatic logic - =, +, <, >, (), etc

**SQLi**

Now, Navigate to account login page in juiceshop.

Try to do SQLi and see if you can get the access to the admin account, 

In the backend the SQL Query works somewhat like, **SELECT * FROM Users WHERE email='test' AND Password = xyz AND DeletedAt is NULL.** i.e account is not deleted, and checks if its an active account.

In the email , type -> **test** -> it shows invalid email/password
then type -> **test '** -> this extra apostrophe is an illogical thing, as it never ends, i.e it doesnt have a closing qoute, so the DB throws back an error, if we capture this in the BS, and do this from the repeater, you can see all other sorts of backend information errors, this DB is throwing which can be really helpful.

Then what you can do is -> type -> **test ' OR 1=1; --** -> this is inputting an OR Condition in our SQL Query, i.e commenting out every other check the backend DB does to validate the authenticity of the user, hence it just sees that the 1=1 condition is true, and it allows the admin access - this is because, typically the indexing done in the backend, ID 1 is usually assigned to the administrator, and when that condition falls true, we are given the admin access.

**SQLi Defenses**

**Defense 1: Parameterized Statements/Queries** - it ensures the inputs are issued safely in SQL Statements.
Eg: **"SELECT * FROM users WHERE email =?";** - here, the parameterized strings will be passed separately than the parameters being provided. 
But one can still evade this by statement like this: **SELECT * FROM users WHERE email = "" + email + "";** - so proper parameterized queries should be coded.

**Defense 2: Sanitizing input:** i.e statements like 1=1 should be sanitized and not allowed. you should have single qoutes in there, etc.

{%highlight ruby%}2. Broken Authentication:{%endhighlight%}

That is using techniques such as , 
**credential stuffing**

**bruteforce attacks (i.e a rate-limiting feature should be there)**

**use of weak/default passwords(Also comes under Sensitive Data Exposure OWASP Category)**

**forgot passwords have knowledge-based questions etc**

**plain text or weakly hashed password stored**

**has missing MFA**

**Exposes Session IDs in the URL and/or Does not rotate session ID after successfull logins (Called as **Session Fixation**)**

**User sessions or tokens (i.e SSO Tokens) are not properly invalidated during logout or period of inactivity.**

**Defenses are below:** - pretty straight-forward:

![BrokenAuthentication Defenses](https://raw.githubusercontent.com/Byteman0xD/Byteman0xD/main/assets/BrokenAuthentication.png)

We will logout from juiceshop and go to the login page 
we do some username/password enumeration, and we see **invalid username or password** -> this is actually good, because some of the websites say invalid username , or incorrect password, which gives the other one away, considered to be a low level information leakage vulnerability.

If we go to forgot password, and we try with known and unknown usernames, we can see the difference there as well, security questions pop-up only when a username is valid, otherwise they are greyed out. - this is also username enuemeration.

We can create a user account, log in and reload the page, capture it in burp, see the details, the token etc, you can log back out, refresh the page, capture it in burp and see if you still see the token or not, you can also check if the token is changed everytime you log in or not, there are lot of things you can do with tokens, broken authentication mainly focuses on session fixation, which does not seem to be a problem here.

{%highlight ruby%}3. Sensitive Data Exposure:{%endhighlight%}

Imagine you are testing against a hospital website, and it is exposing PII or any other client personal information.

Another example can be an exposure of a backup directory having zip files of the whole website including the source code etc.
look for open directories, critical files, critical information etc.

look at **response headers in burp**, we can look at what kind of protections are in place

You can go to [securityheaders](https://securityheaders.com/) and look for websites headers that are included in responses, this is sensitive data exposure at a low level, but if headers like HSTS is missing, the severity can rank up.

We should also be looking at the level of encryption the website is using , we can do that by using NMAP.

Command like: **nmap --script=ssl-enum=ciphers -p 443 tesla.com**

Exposure of headers or use of weak encyrption is not basically a sensitive data exposure, but using such weaknesses we can perform downgrade attacks on ssl, making a way to compromise the weak ciphers in use or maybe the lack of important headers like HSTS can also aid in downgrade attacks - which can then lead to sensitive data exposure of a higher severity.

Simply usage of weak ciphers or unused headers comes under low severity , but it is reportable.

{%highlight ruby%}4. XML External Entities:{%endhighlight%}

XXE Abuses or attacks systems that parses XML input, to get some information out of the system that we are attacking, we can do attacks like DOS, Local file disclosure, RCE, etc.

Few basics of XML, a sample code is below:

<?xml version="1.0" encoding="ISO-8859-1"?> // This is called as the metadata, a sort of declaration that this is an XML Document.
<gift> //this is what is called as the root element , similar to html , the opening and closing of elements.
	<To>Frank</To> //these are called as children elements
	<From>Usman</From>
	<Item>Pokemon Cards</Item>
</gift>

Now, lets suppose there are alot of gifts that i want to give, and i dont want to write them down again and again, in cases like such we use something called as the "Entity"
Entity is like a variable, so to add an entity we say something like this:

>!DOCTYPE gift [  /// declaring the place where to use the entity, in this case it is to be used in the document type **where the root element is gift.**
		<!ENTITY from "Usman"> /// so, here we declare the **ENTITY** to be **from**, and its value to be **Usman**, so wherever we call the variable from, Usman will placed there. 
	]>

we can include the above code, like this:
**<?xml="1.0" encoding="ISO-8859-1"?>**
**<!DOCTYPE gift [**
		**<!ENTITY from "Usman">**
  **]>**
**<gift>**
	<To>Frank</To>
 	<From>**&from;**</From>
	<Item>Pokemon Cards</Item>
**</gift>**

Now, the doctype that we wrote is what is called as the **DocumentTypeDefinition**, famously abbreviated as the **DTD**  and we declared our entity within the DTD.

Now, what if i want to send the gifts from John and steve, both of them.

When we add something like John/Steve or John&Steve in the rootelement inside the code. What happens is that, most of these characters are forbidden characters, we can put in alphanumeric but when it comes to special characters, it kind of gets a little funky. so a special character like &, $, #, >, < will mess things up.

So, what we can do is we can use these entities to call things down
so, we can declare John&Steve in the doctype above as an entity and then call them down in the code - that would work.

so, it works as a variable; But, it also allows us to include other items, like special characters etc.

Which means that we can add things like forwardslashes, colon, something like c:\ etc, to get a file out.

![XXE Example](https://raw.githubusercontent.com/Byteman0xD/Byteman0xD/main/assets/XXEexample.png)

ref: [https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/XXE%20Injection](https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/XXE%20Injection)

so if we copy a sample payload from the above reference repo like below:

<?xml version="1.0" encoding="ISO-8859-1"?>
 <!DOCTYPE foo [  
 		 <!ELEMENT foo ANY >
 		 <!ENTITY xxe SYSTEM "file:///etc/passwd" >
 ]>
<foo>&xxe;</foo>
  
the sample code can be clearly viewed here:

![xxesamplecode](https://raw.githubusercontent.com/Byteman0xD/Byteman0xD/main/assets/xxeattackexample.jpg)

so, here we have our Doctype, and we declare an element inside the DTD, and we also have an entity named XXE and we are calling out the SYSTEM here, before we just declared entity name value, but here we are adding something extra that is SYSTEM. SYSTEM is a keyword used in an entity to let the parser know that the resource is external and should be stored inside the entity - which allows us to put a malicious content inside it - what also the SYSTEM does is that it allows us to pull data from the SYSTEM itself, so what we are trying to do is, to pull the etc/passwd file from the SYSTEM. and then we have our child element, of foo, and calling our variable with &xxe, which is just a placeholder for our file:///etc/passwd command. 

**XXE Attacks and Defenses**

Now we can save our exploit with a .xml extension to use it as our payload for attack.

This attack will not work for juiceshop, as it doesnt work when its hosted on docker. 

So the way we can do this, try to mostly spot places where you can upload documents. even if we dont get a shell or anything, can we bypass it from what its trying to do.
for eg. the upload should only accept jpeg or pdf file but we try to upload the xml file .

And if the exploit worked, you could see the passwd file .

How to defend against this: we need to disable completely the DTDs.

Wether you are able to exploit using xxe injection or not, bypassing the upload feature is in itself a finding which can be reported.

{%highlight ruby%}5. Broken Access Control:{%endhighlight%}

There are so many different vulnerabilities that can fall into this category.
This category is simple a use-case where user gets access to somewhere they shouldnt. - as simple as that.

![brokenaccesscontrol](https://raw.githubusercontent.com/Byteman0xD/Byteman0xD/main/assets/Brokenaccesscontrol.png)

We can do this in juiceshop, by leaving a feedback comment in another users name.

You can login from your test account, go to the feedback page 
we write a comment, give rating and answer the captcha, then we go to **inspect**, and look for the field close to our email field, where it says **hidden**, we can just **delete** that field, and we can see our **user-id** -> this is true with real-life websites even with some big vendors ones as well, proper access control measures are done, instead they just hide from the end-user, which can easily seen using the inspect feature of our browser.

![brokenaccesscontrol2](https://raw.githubusercontent.com/Byteman0xD/Byteman0xD/main/assets/Brokenaccesscontrol2.png.jpg)

There can field in such places like, **hidden** as shown above, or **field=password** where you can change password to text and the password is then visible to you.

This is a clear indication of an ability to bypass access control features, hence the borken access control category.

{%highlight ruby%}6. Security Misconfigurations:{%endhighlight%}

This is also sort of like many of the above, this is just a catch-all category.

like use of default creds, weak creds, stack tracing (the errors you get which might reveal critical info, like while doing the SQLi)

![SecurityMisconfigurations](https://raw.githubusercontent.com/Byteman0xD/Byteman0xD/main/assets/securitymisconfigurations.jpg)

{%highlight ruby%}7. Cross-site Scripting:{%endhighlight%}

There are three type of XSS, shown below:

![XSS](https://raw.githubusercontent.com/Byteman0xD/Byteman0xD/main/assets/XSS.jpg)

Reflected: server reads the request and responds back with that request onto the browser, reflecting on the page. If we leave the page and come back later, it will go away. Unless we make that request again.

Persistent/Stored: Here, we inject a malicious payload and then it is stored onto the server, so, even if we leave and come back the payload is still there..

DOM XSS: Document Object Model, this is in javascript, so when you visit a website, it shows you some html, some javascript it sends it to your browser - when you wanna modify that content - its called the DOM attack.

we can exploit the DOM specifically. DOM and reflected are both client-side attacks.

lets say we have a php page, and we just call it index.php, the code looks like below:

<?php
$username=$_GET['Username']
echo "Hi $username!";
?>

Suppose the URL looks something like, 

**index.php?username=usman**
its gonna say "Hi Usman" - when you come to the webpage.

now, what if we have something like:

index.php?username=**script alert(document.cookie) script**
 -> thats going to run javascript, and give us an alert pop-up box on the browser, and its gonna say '1' on the pop-up -> that is what reflective looks like. script tags are under <> and </>

For stored, think about having a blog post, forum, and you can go and comment on those, what if you leave the above script tags on that comment field, and if the site is vulnerable -> so if everytime, if you or anyone else goes to that page, they can see the pop-up as well.

Reflective and DOM requires Social engineering -> we can send someone a malicious link -> that malicious link might point to us, we might have some javascript running, gonna steal their cookie and send it to us, so, we need to know the user, send that crafted link and then get the cookie.

For Stored -> we can do the same thing, without even doing social engineering -> because the malicious code is just sitting thier on the server.

With XSS, we can perform DOS, deface a webpage, cookie stealing, keylogging etc.

The most common and the best example is stealing cookie and showing pop-up as a POC of the vulnerability.

Below is a blog-post on DOM XSS, its a bit hard to get the mind around, is a little complex -> the blogpost explains it really well.

[https://www.scip.ch/en/?labs.20171214](https://www.scip.ch/en/?labs.20171214)

In DOM, we have a **source** and a **Sink** - Source is where your malicious code goes into, and Sink is where it will get executed.

Read the full article to get a good understanding of DOM Based XSS.

Also a game for practicing your XSS skills - [https://xss-game.appspot.com/](https://xss-game.appspot.com/)

![XSS2](https://raw.githubusercontent.com/Byteman0xD/Byteman0xD/main/assets/XSS2.jpg)

{%highlight ruby%}8. Insecure Deserialization:{%endhighlight%}

Serialization is taking the data, and convert the object to a format which can be put on to a disk and then can be sent over to a network or through a network. It can be serialized in XML, JSON, YAML, Binary etc - it is serialized and sent over.

Deserialization: So, we deserialize the serialized malicious input that we use.

i.e we create a payload, we serialize it and put it on the target area, the server then desrializes it and eventually executes our payload.

These are kind of hard to find

Read through the owasp top10 to get more clarity on this.

Juiceshop does not have a good example for this.

read up on **ysoserial** , a tool which people use quite a bit for finding this sort of bug.

[https://github.com/frohoff/ysoserial](https://github.com/frohoff/ysoserial)

read up on this, to understand what it does and how to use it.

This vulnerability is something which you wont come across a lot, but it still belongs to the owasp top 10, because it can be very deadly.

{%highlight ruby%}9. Using Components with Known Vulnerabilities:{%endhighlight%}

You are gonna see this, when it comes to network pentesting, like the eternal blue.
You can see these in webapps as well. You can use different tools you can use to find these known vulnerabilities, out-of-date kind of applications etc.

In burpsuite, if you have the pro version, you can use the active scan feature, and from the Bapp store, you can use tools like **active scan++, retire.js (looks potential vulnerable javascript libraries), software vulnerabiilty scanner, java deserialization scanner, additional scanner checks, software version reporter** etc, there are many other bunch of tools - these are all part of the burp pro version.

You can also use tools like wappalyzer to see the technology and language the website is running and look up on it, if its on the updated version or not etc.

you can also run nessus against the website, ports on that server, etc.

you can also go on shodan, where you can go up there and look up different vulnerabilites, for eg. looking machine still vulnerable to apache struts 2.

An active patching schedule inside an organization is a must to not be a target of known vulnerabilities by a hacker.

{%highlight ruby%}10. Insufficient Logging and Monitoring:{%endhighlight%}

pretty much self-understood.

i.e having visibility of all sorts of logs from your network, devices, websites, machines etc. 

incorrect logins, bruteforce, user login, etc, the monitoring should be alerted whenever anything happens. There should be a monitoring team in the organization.

When you do pentesting for a company - this can even go into the report like -> i was doing so and so penetration tests, and no one from the team even saw me do this let alone catch me -> insufficient logging and monitoring.