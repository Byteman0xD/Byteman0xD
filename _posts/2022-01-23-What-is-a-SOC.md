---
layout: post
title:  "Security Operations Centre"
#date:   2022-01-23 17:03:35
category: general
---
Security Operations Centre

Security Operation is the continuous operational practice for maintaining and managing a secure IT environment through the Implementation and execution of certain services and process. Its main purpose is to detect, prevent, prioritize and respond to security incidents.

Security Operation Consists of various security operation tasks, like:
•	Security Monitoring
•	Security Incident Management
•	Vulnerability Management
•	Security Device Management
•	Network flow Monitoring

SOC is a centralized unit and a single point of view through which an organization’s assets are monitored, assessed, and defended from the threats. It also facilitates situational awareness and real-time alerting if any intrusion or attack is detected. It functions as a hub or central command post, collecting telemetry from across an organization's IT infrastructure, including networks, devices, appliances, and data stores, regardless of where those assets are located. The proliferation of advanced threats involves gathering context from a variety of sources. Essentially, the SOC serves as a point of contact for all events logged within the organisation that are being monitored. The SOC must determine how each of these events will be managed and dealt with.


Traditionally, SOCs have been built on a hub-and-spoke architecture, with a security information and event management (SIEM) system aggregating and correlating data from security feeds. Vulnerability assessment solutions, governance, risk, and compliance (GRC) systems, application and database scanners, intrusion prevention systems (IPS), user and entity behaviour analytics (UEBA), endpoint detection and remediation (EDR), and threat intelligence platforms (TIP) are all examples of spokes in this model .

The SOC is typically led by a SOC manager and may include incident responders, SOC Analysts (levels 1, 2, and 3), threat hunters, and an incident response manager (s).The SOC reports to the CISO, who in turn reports to either the CIO or directly to the CEO.

{% highlight ruby %}Roles in a SOC Department:{% endhighlight %}

Both the security technologies (e.g., software) and the individuals who make up the SOC team provide the “framework” for your security operations.

Members of the SOC Team Include:
1. Manager: The group's leader can fill any function while also monitoring the security systems and processes.

2. Analysts: They compile and analyse data from a specific time period (for example, the preceding quarter) or after a data breach.

3. Investigator: After a breach, the investigator works closely with the responder to figure out what happened and why (typically, one individual serves as both investigator and responder).

4. Responder: Responding to a security breach entails a number of responsibilities. During a crisis, someone who is familiar with these requirements is essential.

5. Auditor: All current and future legislation includes compliance requirements. This position keeps track of these standards and ensures that your company meets them.

Note: Depending on the size of an organization, one person may perform multiple roles listed. In some cases, it may come down to one or two people for the entire “team.”

{% highlight ruby %}SOC capabilities{% endhighlight %}

Situational Awareness and operational intelligence - SOC provides information about what is going on across the different parts of IT infrastructure. It also provides Operational intelligence about IT infrastructure.

Threat control and prevention - Using Internal and external resources, it provides the knowledge of IOC'S (Indicator of compromise) of attacks. This enables SOC to provide Threat control and prevention.
Forensics - SOC analysts use structured log data to conduct investigation and understand the root cause of the attack and restrict the attacker's ability to perform an attack against the organization.

{% highlight ruby %}10 key functions performed by the SOC:{% endhighlight %}

1.Take Stock of Available Resources

The SOC is in charge of two types of assets: the various devices, processes, and applications they are tasked with protecting, as well as the defensive tools at their disposal to assist in this protection.

- What The SOC Protects: The SOC cannot protect devices and data that are not visible to them. There are likely to be blind spots in the network security posture that can be discovered and exploited if visibility and control from device to cloud are not provided. As a result, the SOC's goal is to obtain a comprehensive picture of the business' threat landscape, which includes not only the various types of endpoints, servers, and software on premises, but also third-party services and traffic flowing between these assets.

- How The SOC Protects: The SOC should also have a thorough understanding of all cybersecurity tools available as well as all SOC workflows. This improves agility and enables the SOC to operate at maximum efficiency.

2.Preparation and Preventative Maintenance

Even the most well-equipped and agile response processes are ineffective when it comes to preventing problems from occurring in the first place. The SOC implements preventative measures, which can be divided into two categories, to help keep attackers at bay.

- Preparation: Members of the team should stay up to date on the latest security advances, cybercrime trends, and the emergence of new dangers on the horizon. This study can be used to develop a security roadmap that will guide the company's cybersecurity activities in the future, as well as a disaster recovery plan that will provide ready guidance in the event of a worst-case scenario.

- Preventative Maintenance: All activities done to make successful attacks more difficult, such as regularly maintaining and updating existing systems, updating firewall policies, patching vulnerabilities, and whitelisting, blacklisting, and securing apps, are included in this phase.

3.Continuous Proactive Monitoring

The SOC's tools scan the network 24 hours a day, seven days a week, looking for any anomalies or suspicious activity. The SOC can be warned of developing risks promptly by monitoring the network around the clock, providing them the best chance to avoid or mitigate harm. A SIEM or an EDR are examples of monitoring technologies. The most advanced of these can employ behavioural analysis to "teach" systems the difference between normal day-to-day operations and genuine threat behaviour, reducing the amount of triage and analysis required by people.

4.Alert Ranking and Management

When monitoring tools send out alerts, it's up to the SOC to examine each one carefully, delete any false positives, and decide how aggressive any actual threats are and what they might be targeting. This enables them to effectively prioritise emerging threats, addressing the most pressing concerns first.

5.Threat Response

When most people think of the SOC, they think of these acts. The SOC responds as a first response as soon as an incident is confirmed, shutting down or isolating endpoints, stopping malicious programmes (or blocking them from executing), deleting files, and so on. The goal is to respond to the extent required while minimising the impact on company continuity.

6.Recovery and Remediation

Following an incident, the SOC will try to restore systems and recover any data that has been lost or compromised. Wiping and restarting endpoints, restructuring systems, or, in the case of ransomware attacks, establishing valid backups are all possible ways to avoid the malware. This step, if completed successfully, will restore the network to its pre-incident state.

7.Log Management

The SOC is in charge of gathering, maintaining, and reviewing all network activity and communications logs for the whole enterprise. This information helps create a baseline for “normal” network activity, can show the presence of risks, and can be utilised for post-incident remediation and forensics. Many SOCs employ a SIEM to collect and correlate data from applications, firewalls, operating systems, and endpoints, all of which generate their own logs.

8.Root Cause Analysis (RCA)

Following an incident, the SOC is in charge of determining exactly what happened, when, how, and why. During this inquiry, the SOC will use log data and other information to trace the problem back to its source and create an RCA Report, which will aid in the prevention of future problems.

9.Security Refinement and Improvement

Cybercriminals are continually improving their TTPs (Tools, Techniques and Procedures) and the SOC must keep up with them by implementing upgrades on a regular basis. The strategies established in the Security Road Map are brought to life in this step, but it can also involve hands-on techniques like red-teaming and purple-teaming.

10.Compliance Management

Many of the SOC's processes are guided by best practises, but others are driven by regulatory obligations. The SOC is in charge of auditing their systems on a regular basis to ensure compliance with any requirements imposed by their company, industry, or regulating authorities. GDPR, HIPAA, and PCI DSS are examples of these regulations. Acting in line with these regulations not only protects the sensitive data entrusted to the firm, but it also protects the company from reputational harm and legal problems that may arise as a result of a breach.

{% highlight ruby %}Optimization of a SOC:{% endhighlight %}

While incident response consumes the majority of the SOC's resources, the chief information security officer (CISO) is accountable for the overall risk and compliance picture. An effective strategy involves an adaptive security architecture that enables organisations to conduct optimum security operations to bridge operational and data silos across various activities. This method improves your information security management posture while increasing efficiency through integration, automation, and orchestration. It also minimises the number of personnel hours necessary.

Adoption of a security framework that makes it easy to integrate security solutions and threat intelligence into day-to-day procedures is required for an efficient security operations model. Threat data is integrated into security monitoring dashboards and reports using SOC tools including consolidated and responsive dashboards, which keeps operations and management up to date on changing events and actions. SOC teams can better manage overall risk posture by connecting threat management with other tools for risk and compliance management. Such configurations provide continuous visibility across systems and domains, as well as the utilisation of actionable intelligence to improve security operations accuracy and consistency.

 
