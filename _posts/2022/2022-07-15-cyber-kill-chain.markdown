---
layout: "post"
title: "The Cyber Kill Chain"
categories: [Learning, Cybersecurity, IT, SOC]
tags: [Learning, Cybersecurity, IT, SOC]
date: "2022-07-16 09:43"
---
Shigraf Aijaz put up an article, [How can SOC analysts use the cyber kill chain?](https://cybersecurity.att.com/blogs/security-essentials/how-can-soc-analysts-use-the-cyber-kill-chain). The [cyber kill chain](https://www.lockheedmartin.com/en-us/capabilities/cyber/cyber-kill-chain.html) was developed for identifying and preventing cyber-attacks and data exfiltration. It lists out seven steps normally involved in a cyber-attack:

1. Reconnaissance
2. Weaponization 
3. Delivery 
4. Exploitation 
5. Installation 
6. Command and control (C2) 
7. Actions on objectives

## 1\. Reconnaissance
Researching the target, what information is publicly available and what can be done to monitor and mitigate potential vulnerabilities? Monitor for port scanning, use threat intelligence and network intrusion detection systems (IDS), Firewall, access control, VPN and more.

## 2\. Weaponization
Preparation and staging phase. No direct interaction, instead preparing for attack. Endpoint malware detection, proxy filtering, application whitelisting, app-aware firewalls, network intrusion prevention system (IPS) etc.

## 3\. Delivery
Infiltration of network through phishing email, hacking digital/physical and so forth. Ensure endpoint security through robust anti-malware software, anti-phishing software, user education, zero-trust security model, firewalls.

## 4\. Exploitation
Actual attack, targeting of application or operation system vulnerability for exploitation to gain access. At this point, assume that malicious payload has been deployed triggering exploit. Endpoint malware protection and a host-based IDS. Efficient and timely patch management and secure password policies. Containment through app-aware firewalls and inter-zone network IDS.

## 5\. Installation
Exploit occurring within the target system, orientation, pivoting, lateral movement, detection of vulnerabilities to exploit. Privilege escalation, and persistence through backdoor or remote access trojan (RAT). Security Information and Event Management (SIEM) and a host-based IDS. Inter-Zone Network Detection System, trust zones, and an App-aware firewalls for containment of compromised systems. Strong passwords, multi-factor authentication for endpoints, and privilege separation practices.

## 6\. Command and control (C2)
An adversary controlled server used to send commands to, or receive data from the hacked system. Often cloud-based services for file-sharing. Host-based or network IDS, network segmentation, access control lists (ACLs), and firewalls, trust zones and domain name system sinkholes.

## 7\. Actions on objectives
Main objective, distributing malware, conducting a denial of service (DoS) or distributed DoS (DDoS) attacks, deploying ransomware etc. Endpoint malware protection and data-at-rest encryption to mitigate the attack and develop a robust incidence response plan.