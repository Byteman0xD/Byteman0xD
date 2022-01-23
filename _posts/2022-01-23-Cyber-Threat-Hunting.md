---
layout: post
title:  "Cyber Threat Hunting"
#date:   2022-01-23 17:03:35
category: general
---
Cyber Threat Hunting

“Never expect machines to be ethical or strategic. Never expect humans to be good at searching large volumes of data at speed and scale or perform complex pattern matching”.

Cyber threat hunting is a proactive security search of networks, endpoints, and datasets for malicious, suspicious, or risky actions that have eluded existing detection systems. As a result, there is a distinction to be made between cyber threat detection and cyber threat hunting. Threat detection is a more passive method of monitoring data and systems for potential security flaws, but it's still necessary and can help a threat hunter. Proactive cyber threat hunting strategies have evolved to identify and categorise possible threats in advance of an attack by combining new threat intelligence with previously obtained data.

Threat hunting can mean slightly different things to different organizations and analysts. Whatever interpretation is used, it's vital to remember that threat hunting takes a lot of time because finding items of interest is much more difficult when signatures aren't available. Furthermore, it is more important to ensure that enterprises and their analysts are constantly conducting threat hunting by ensuring they can detect and remediate any cyber hazards, rather than the semantics of the term.

A 2019 Forrester Consulting study, commissioned on behalf of McAfee, revealed the top two endpoint security goals and initiatives of decision makers were to improve security detection capabilities (87%) and to increase efficiency in the SOC (75%). According to the study, gaps in EDR capabilities have created pain points for 83% of enterprises.


{% highlight ruby %}Threat Hunting Techniques:{% endhighlight %}

1. Supposition: Threat hunts begin with a hypothesis or assertion about the hunter's perceptions of potential hazards in the environment and how to find them. The strategies, techniques, and processes (TTPs) of a suspected attacker can all be included in this Supposition. Threat hunters construct a logical path to detection using threat intelligence, environmental knowledge, and their own expertise and inventiveness.

2. Searching: This entails searching evidence data for specified artefacts using well stated search criteria, such as full packet data, flow records, logs, alerts, system events, digital pictures, and memory dumps. Because it's rare to know exactly what to look for while searching for threats, it's critical to strike a balance between not making search criteria too broad (i.e. becoming overwhelmed by too many results) and not making them too narrow (i.e. becoming overwhelmed by too few results) (i.e. missing out on threats by receiving too few results).

3. Clustering Data: Clustering is extracting groups of comparable data points based on specific characteristics from a larger data set using machine learning and AI technology. Analysts can use the technique to acquire a broader view of data that's of interest, uncover commonalities and/or unrelated connections, then weave those insights together to get a fuller picture of what's going on within their organization's network and figure out what to do next.

4. Tuning artefacts: Taking numerous unique artefacts and determining when multiples of them appear together based on specified search criteria is known as grouping/Tuning.

5. Artefact Stacking: This approach is counting the number of occurrences for values of a specific type of data and examining the outliers. When data sets yield a finite number of results and inputs are appropriately designed, stacking is most effective. Finding abnormalities in huge data sets requires the ability to organise, filter, and edit the data in question, thus using technology — even something as simple as Excel — is crucial when stacking.

6. Intelligence, Trigger, and Investigation (ITI): An Intelligence strategy for gathering, storing, and processing data is required. SIEM (Security Information and Event Management) software can provide visibility and a history of activity in an organization's IT environment. When sophisticated detection tools guide threat hunters to a specific system or area of a network, a hypothesis might act as a trigger to start an inquiry. Endpoint Detection and Response (EDR) technology, for example, can hunt, search and investigate deep into potentially dangerous abnormalities in a system or network, finally determining whether they are benign or malicious.

7. Remediation: To respond to, resolve, and mitigate threats, data obtained from confirmed malicious activity can be fed into automated security technology. Remove malware files, restore altered or deleted files to their original state, update firewall/IPS rules, install security patches, and change system configurations – all while learning more about what happened and how to strengthen your protection against such assaults in the future.

{% highlight ruby %}Optimizing Cyber Threat Hunting:{% endhighlight %}
 
The Most feasible optimization of time, resource and investigation is done when Human expertise and Machine expertise Team-up to get to a conclusion. Humans assist in reaching a resolution faster and with greater accuracy, as well as removing redundant and routine manual errors that can be riddled with mistakes.

{% highlight ruby %}Key Threat Hunting Take-aways:{% endhighlight %}

•	Proactive: Rather than waiting for an alert from an existing security tool, threat hunting requires proactively sniffing out potential intruders before any alerts are generated.

•	Trusting your Gut: The best threat hunters avoid relying too heavily on alerts coming from automated security technologies. Instead, they look for clues and listen to their gut, and eventually apply those findings to create automated threat detection rules.

•	Tracking Traces: The Whole Idea of Threat Hunting revolves around  believing that an organization's environment has been compromised and that attackers have left traces. It is therefore critical to follow all traces and clues to the end, no matter how long or winding the hunt may be.

•	Embracing Creativity: Hunting for threats isn't about sticking to the rules. Threat hunting necessitates embracing imagination as well as any appropriate approaches in order to keep ahead of the most talented and imaginative adversaries.

However, it is also clear based on these characteristics that many organizations find it difficult to establish a well-skilled threat hunting team. 

Instead, it becomes a work of art that only one or two individuals are capable of and even they require enormous investment of time.

This lack of repeatability stems from a lack of support for this process. Even the most proficient threat hunters struggle to consistently produce valuable results.
