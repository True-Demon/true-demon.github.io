---
title: Projects
description: Projects List
permalink: /projects/
---

## King Phisher
**Description:** Attack-centric Email Phishing platform originally developed by SecureState LLC. <br />
**Language:** Python (mostly)<br />
**Contributions:** Message Padding plugin, SMTP Notifications via SMTP2GO <br />
**Links:** 
  - [Main Repo](https://github.com/securestate/king-phisher)
  - [Plugins Repo](https://github.com/securestate/king-phisher-plugins)
  - [Templates Repo](https://github.com/securestate/king-phisher-templates)
  - [Developer Documentation](https://king-phisher.readthedocs.io/en/latest)
  - [Wiki Page](https://github.com/securestate/king-phisher/wiki)


>King Phisher is an attacker-focused penetration testing tool that assists in creating convincing, sneaky little phishing emails in a handful of different ways. The project aims to aid attackers in breaching the perimeter in ways that no other tool can claim to do. The tool is a server/client pair that operates by combining web and mail services, and consolidating/simplifying DNS. <br /><br />
>The server is built to host phishing websites based on templates and are highly customizable with jinja tags. The server also sends the phishing emails on behalf of the client, along with capturing credentials, dropping malicious links & payloads, and notifying the attacker when there is traffic/success from the phish.<br /><br />
>The client-side application communicates with the server to set up the email itself, allowing the attacker to rapidly create a convincing HTML-formatted email phish or calendar invitation. Additional features include a plugin-based feature system, including convenience add-ons such as the various notifications plugins, and phishing/stealth plugins such as the message padding and tracking-dot images to bypass spam filters & detect when emails are opened. <br /><br />
>All in all, King Phisher is the largest project I've contributed to, as well as the most enjoyable learning experience.

## Raindance
**Description:** Raindance is a reconnaissance tool for enumerating user data from Office 365
**Language:** Powershell
**Contributions:** Sole Developer
**Links:**
  - [Main Repo](https://github.com/true-demon/Raindance)

> Raindance is a reconnaisance and enumeration tool which targets Office 365 for useful information. The main use for Raindance is acquiring a list of valid users, email addresses, mailing groups, and permissions from Office 365 using a valid set of user credentials. Because it is common to acquire user credentials from either phishing or password-guessing while on engagements, raindance can be used to gather additional information that would be otherwise unobtainable. This is especially useful when VPN services or remote-access portals are unreachable with the current user credentials and more information is needed to launch external attacks. <br/><br/>
> I began developing Raindance at the encouragement of some of my colleagues at SecureState, after assisting with a penetration test by using some helpful Powershell-fu to obtain user information from Office 365. Because powershell allows for a built-in, default, and largely unmonitored method for accessing Office 365 data, it is ideal for quietly extracting information about an organization without ever touching the domain. <br/><br/>
> Raindance was presented at BSides Cleveland in June 2018 to help illustrate some of the usable data that can be pulled out of it, and ways blue-team can detect and prevent users connecting via powershell to pull out data.
<iframe width="350" height="210" src="https://www.youtube.com/embed/VHPZ2YU351M" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe> 
