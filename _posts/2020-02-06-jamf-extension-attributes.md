---
layout: post
title: "Install and Running Status - Jamf Extension Attributes"
date: 2020-02-06
excerpt: "Find out whether any app is up and running through jamf"
tags: [jamf, sysadmin, paulipotter]
---

---
As a jamf admin, I've found myself needing to know whether a certain app, not present in the /Applications folder is installed or running. These two snippets are perfect for this purpose.

#### Install Status
For this one, I search for certain file(s) using bash's if statement. I will very often check either the /LaunchDaemons, /LaunchAgents folder or a certain file under /Library/<application-name>

Install Status: Bomgar Jump Client
{% highlight ca65 %}
#!/bin/bash
bomgarLaunchAgent=`ls /Library/LaunchAgents/ | grep com.bomgar.bomgar-scc`
if [[ $bomgarLaunchAgent != '' ]]; then
  echo "<result>Installed</result>"
else
  echo "<result>Not Installed</result>"
fi
exit 0
{% endhighlight %}

#### Running Status
I've used this type of scripts to troubleshoot programs that run in the background and are not easy to find out whether they are doing what they're supposed to. This is perfect to monitor Vulnerability Agents, MDM Clients or anything that's not too obvious for end-users that you need running.

Example: Running Status - Nessus Agent
{% highlight ca65 %}
#!/bin/sh
NessusAgentRunning="$(sudo launchctl list com.tenablesecurity.nessusagent | grep "PID" | awk '{ print $1 }' | tr -d '\"')"
if [ "$NessusAgentRunning" = "PID" ]
  echo "<result>Running</result>"
else
  echo "<result>Stopped</result>"
fi
exit 0
{% endhighlight %}
Nessus Agent is a vulnerability scanner that runs in the background to find and prioritize the necessary patches and any other vulnerability. It is easy to install but this way you can make sure that's running and connected to the server.
