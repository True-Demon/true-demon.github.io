---
layout: post
title:  "Raindance: Raining Recon from the Microsoft Cloud"
date:   2018-06-26
categories: tools osint
---

If you've been performing penetration tests for the last several months, then you may have encountered Office 365 in your travels. Office 365 (O365) for the sake of clarity, is the latest and greatest Cloud solution for various popular Microsoft services including Exchange, Sharepoint, Database management, and a cloud-based active directory serviced called Azure Active Directory. It's a great service for businesses of all sizes, except that it just a bit too user-friendly for its own good.

O365 started with its initial release in June 2011 and has grown into one of the most widely used enterprise cloud platforms. The beauty of it was the ability for all of these services to be made available through the cloud, meaning publicly/remotely accessible, configurable, and manageable. The unfortunate downside is the security implications this has.

## Story time
After discovering in 2015 that Office 365 could be managed through Powershell, my job became learning how to make my life easier by using Powershell to automate anything and everything I could in the platform. Fast-forward 2 years later after having shifted my career path exclusively to red-teaming and network penetration, it became my target.

A colleague of mine was on a pentest, and while observing him work, he became stuck after acquiring user credentials and having no VPN or login portal to use them on. All he had was email, which wasn't quite what he was hoping for. Remembering that I knew how to log into Office 365 with Powershell, I offered to help him out by getting him a list of domain users to do with what he pleased.

After I explained a few brief Powershell commands and helped him install the necessary modules, we logged into their O365 tenant via powershell and I ran `Get-MsolUser -All` against it. Like clockwork, Powershell started dumping out the entire list of domain users. Not only that, it gave up all of their email aliases, mobile numbers, office locations, job titles, and just a metric ton of useful information he could use to craft a **VERY** elaborate internal phishing email.

I walked away from that engagement thinking to myself,

> "Man, it would be awesome if we had a tool that did all that for us. That way we wouldn't need to know all these obscure Powershell commands."

Well, such a tool didn't exist, and I had a *lot* of free time on my hands, so I set to building my first open-source project, [Raindance](https://github.com/True-Demon/Raindance).

## Crafting the Tool
![Raindance-Title](/images/raindance/Raindance-Title.png)
Building RainDance was relatively short and simple while I had access to a working cloud instance of O365. The Powershell commands for the MSOnline and AzureAD modules are very well documented, and I had some cursory working knowledge of the commands before hand thanks to my work experience with Office 365 as a Cloud Admin those few years ago.

However, I quickly learned that accessing the Cloud yielded way more than just usernames. Digging further into the modules, I found many more things that could be pulled out of the Cloud, and over time it continued to grow. What was meant to be maybe a 20-line script turned into a decently sized library of Powershell code that effectively turned user-level O365 access into a reconnaissance gold mine.

Just some things you can pull out of Office 365 with effective working knowledge of Powershell:

* List of domains joined to the O365 tenant
* List of valid subscriptions & software licenses
* All users & their configured properties in O365
  - User principal name
  - Email
  - Mobile & Desk phone numbers
  - Street address, Office, building, room, & in some cases mailbox numbers
  - Device names (& properties) the user has joined to the Cloud
  - Services & permissions given to the user
  - The last time the user changed their password
* All mail distribution & security groups
* All security roles configured within O365
* All *members* of security roles within O365 (including administrators)

You can also pull Email inboxes and search through user Exchange mailboxes. Not only that, I discovered that, with global/company administrator access (Cloud version of Domain Admin), it is possible to impersonate *other users' inboxes* and search through them as well. You can even download an entire user inbox to your local machine over Powershell, or simply search through it on the Exchange Online tenant without ever having any Email data pass over the network.

> Needless to say, this had *ton* of potential.

## Presenting the Tool

While I was working on Raindance in its beta stage, I presented it to my co-workers. It was received very well. It was good enough that several of them suggested that I present it at the next BSides conference that would be taking place. I reluctantly agreed after much convincing on the part of @ZeroSteiner (I'm not one for speaking to crowds), but it was a good opportunity, and I'm glad I followed through.

I had the opportunity to meet @Carnal0Wnage the day before the con, and he did a great job of calming my fears. I was under the impression for the longest time that it would just be another "meh" tool. In fact, he had given me some pretty sweet ideas about what else could be done with the power that Powershell offered to us as attackers in the Cloud. It's a bit early yet to start pulling back curtains and revealing new findings, but I'm optimistic enough to say...they were some pretty frickin' cool ideas. I'll probably throw up another blog about it in the near future once the functionality is implemented.

Anyway, I got the chance to present at BSides Cleveland on June 23, 2018 and it was a hell of an experience. Beers flowing, hackers pwning, good times had by all. The talk went better than I could have hoped for, and I'm looking forward to presenting again. If you would like to catch the video, you can find it on YouTube here on Adrian Crenshaw's channel: [Raindance: Raining Recon from the Microsoft Cloud](https://www.youtube.com/watch?v=VHPZ2YU351M)

## Usage

Raindance is a powershell module that can be imported into any Windows machine with Powershell v5.0 and above. Microsoft has promised that *eventually* they will be implementing support for Powershell Core v6.0 which will be Linux compatible, but for now, Windows is an inherent requirement.

### Installing Raindance
Raindance has a number of dependencies but naturally works within powershell. Start by downloading via github and install any modules as necessary. If you are missing any dependencies, raindance will warn you.

`git clone https://github.com/True-Demon/raindance.git C:\path\to\raindance`

#### Run powershell as Admin, install modules
`install-module msonline`
`install-module AzureAd`
Say 'Yes to All' to the prompts to indicate you want to install all module features.

#### Import Raindance
`Import-Module C:\path\to\raindance\raindance.ps1`

And just like that, you've got access to raindance.

Immediately, raindance will offer a login prompt for you to log into a target O365 tenant. One thing I will point out here is that logging into Office 365 is not an exact science, and there are a lot of things that can get in the way like 2-factor authentication, X.509 certificates, white-listing controls, and those *very* smart admins who decided their users don't need Powershell, and so they disable access to O365 via powershell by default. 

*Psst -- Admins, I hope you're paying attention ;)*

If the login prompt fails the first time, don't give up. Try running `Connect-MsolService` without any parameters and see if logging in manually through the pop-up login window works. If it succeeds, then you can continue to use Raindance.

> NOTE: In the current version of raindance (As of June 26, 2018) there is a global variable set to indicate whether you have had a successful login. This must contain a valid username for Raindance to allow you to continue. If login succeeds, run
`$Global:LOGGED_IN_USER = 'username@target-domain.com'`
This will allow you to proceed.

Run `Rain-Help` to get a list of all the commands available to you. I've taken the time to comment the code somewhat so if you ever feel lost or curious about how things work, just jump in.

![Raindance-Help](/images/raindance/Raindance-Help.png)
*Raindance Help & Usage*

I leave it to the end-user to experiment with the tool and enjoy exploring the powershell that lies beneath. Everything is running commands from the mentioned Powershell modules and as such are being used as intended. These are the same features used by administrators, and by default are not locked down to prevent you from abusing them.

*Go make it Rain :)*

<p style="text-align:right;"><b>-- True Demon</b></p>

