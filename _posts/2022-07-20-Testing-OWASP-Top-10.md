---
layout: post
title:  "Testing OWASP Top 10"
date: 2022-07-19 10:55
category: general
---
To get a basic understanding of OWASP TOP 10, we are going to test the owasp top 10 vulnerabilites on a Vulnerable website/tool called as "JUICESHOP".

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

{%highlight ruby%}SQL Injection Overview:{%endhighlight%}

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

![SQLiExample](assets\SQLiexample.png)


