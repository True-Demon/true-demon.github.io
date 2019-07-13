---
layout: "single"
title: "Red-Team Theory 1: Cash Register Smash n' Grabs"
date: "2019-07-13 15:30"
comments: true
toc: true
tags: [device pentesting, rubber ducky]
---

{% include figure image_path="/images/posts/Smash-and-Grab.jpg" type=center %}

I decided to start a new series dedicated to the Red Team activities I find myself doing as part of my work in penetration testing. Mostly these are for compliance-driven engagements where the client wants us to try to break into something in order to prove it is secure enough for whatever authority they are trying to impress. Other times, they are part of sanctioned Red Team engagements where we go after mature targets and security conscious companies that want and **expect** us to break into their networks. Anything less is just ... well ... disappointing.
<!--more-->

Since some of these have been pretty exciting to work on, I figured I would share them with my internet homies because what's a good hack if you can't share it with your friends?  

Before we get started, I have to throw down the obligatory "I am not responsible for your actions" statement. I do NOT encourage anyone to attempt these techniques on a system they don't own or have permission to test. Doing so is a crime, and you are solely responsible for that. If you decide you want to try to break into a cash register at a local retail store and don't have express written permission to do so, that is **your** decision. If you get arrested trying to use these techniques, that is **your** problem and you deserve the prison time. Do not send cops or a lawyer to my door to me that I encouraged or was complicit to a felony because some stupid lunatic tried to pop a cash register after reading this article. I actively discourage and hold in contempt anyone who attempts to use knowledge of computers to defraud and harm others.

<hr>

**LEGAL DISCLAIMER:**

> <span style="font-size:.6em">This blog post is for informational purposes only. The techniques detailed within are only to be used within the legal context of a sanctioned penetration test and agreed upon between both the owner of the system and the penetration tester(s). This author does not condone or encourage anyone to use these techniques to illegally or without permission to gain access to any computer system. Anyone caught using these techniques for illicit purposes including but not limited to unauthorized computer system access, credit fraud, or wire fraud is guilty in their own right. The author not responsible for any person using the following to commit a crime. By reading further, you agree you will not attempt these techniques to gain access to any system which you do not own or do not have explicit written permission from the owner to perform said actions. By violating this agreement, you admit full responsibility for your own actions and your decision to commit computer fraud against the advice and direction of the author. </span>  

<hr>

Now that that's out of the way, let's get to the fun stuff! :grin:

Recently I've had the pleasure of working on a series of device pentests that focus on what are supposed to be locked down and hardened embedded devices running simple cash register software. Interestingly, most of these devices aren't as locked down as you might assume, and they offer up some really amazing opportunities to compromise entire networks because of their naturally weak configurations.  

If you've read into events like the Saks 5th Avenue and Lord & Taylor store breaches, you might be wondering how an attack can go from one employee terminal all the way to all the stores of a massive, multi-national retail brand. As it turns out, it's a a lot easier than you'd think.  

## Much ado about cash registers and why they suck

So first, let's cover the basics about what these devices actually are. In truth, most cash register devices that I've seen are nothing more than custom PC hardware fit to an attached monitor. Most of these devices are simple ATX or Mini-ATX boards with your standard peripherals. USB or serial I/O keypads, maybe a USB or Parallel ATA interface for a receipt printer, and a few NICs, usually one dedicated to the credit-card readers. Depending on how far back you go in time, you might find almost every peripheral device is on ancient hardware with serial interfaces and proprietary transaction hardware, but almost all of them are fitted with USB ports. Their only real saving grace is that PCI _typically_ requires the register be compatible with a card reader that encrypts and/or tokenizes the data at the point of dip or swipe. At that point, the register's role is to hold cash (duh) and acts like a glorified calculator and barcode reader.  

Because the vendors that build these things are the same companies that build home computers like you and I have, they build them to be modular and general-purpose. They leave it up to third-party software developers to actually create and sell the application-layer for all the cash register software. Everything else like the till, lock bar, receipt printer and in some cases an integrated card reader are just add-on peripherals that operate over the same old fashioned USB, PS/2, and serial interfaces that have been there for years.

If I were to give you three guesses about which flavor of operating system is running on 90% of these, I'm betting Windows would be your first pick, and you'd be right. Software development for Windows is made to be as easy as possible to reach the greatest number of customers. After all, Windows is built to be a _business-friendly_ OS. The documentation for it has been dutifully maintained and extrapolated to the point where anybody that wanted to could simply visit Microsoft Technet and get started writing a fully functioning payment terminal application almost exclusively using the Windows API and a beginner's understanding of C/C++.  

Ironically, sometimes I feel like the developers who built some of these payment terminal applications has _exactly_ that much experience when they pushed some of these things into production. Despite the best efforts of the clients I work with who have to cobble together their own solutions to secure the cash registers, it feels like they dread seeing me every time I walk up to the payment terminal, waiting to hear what I've broken next.  

They really have their work cut out for them ...  

They have to buy hardware that comes pre-fabricated from a vendor with a stock image of Windows Embedded and register software that looks like it could have been written by a team of high school programming students. In many of the cases I've witnessed, the registers need to communicate back to a server that manages them. Once again, the farther you venture back in time for the model of a register, the more outdated and irrelevant the software becomes to actually run it all. If you come from the PCI world, you might have noticed that the credit card overlords at the big 5 founding companies of the PCI DSS standards (AMEX, MasterCard, JCB, Discover and Visa) whack the living daylights out of anybody that _dares_ to use a vulnerable piece of software older than 2008. However, if you look at the software and hardware companies that partner with the big 5 that is actually _maintained and approved for use_ by these credit card companies, some of these monstrosities predate Windows XP.

My brief experience as a PCI Approved Scanning Vendor (ASV) led me to notice a convenient exception that "Point of Sale Systems are permitted to use SSL" if you can prove it is not vulnerable to known exploits for SSL/TLS.

![PCI POS Exception](/images/posts/pci-dss-poi-ssl.png)

Brutal irony and shameless hypocrisy aside, why would PCI make an exception for this specifically? Because most of the cash registers are using outdated Windows operating systems. They are meant to be stripped down, embedded Windows OS that is (supposedly) secured enough that you couldn't possibly intercept the credit card data and feasibly decrypt it. To PCI that's all that matters ... oh, and that they don't have to spend money replacing all their ancient hardware ;)

## The Scenario

In this instance, I was given free-reign by our client to test a point-of-sale system for as long as necessary in order to determine whether it was secure enough for a production environment. Previously, the POS device and software had already been tested and green-lit for PROD, but they wanted to see how secure it actually was in the field where things aren't as squeaky clean as the testing labs.

Realistically, most attackers trying to go after credit card data are not going after point of sale terminals, and they won't have nearly as much time as I did to test the device. Let's be honest, the risk of getting caught is astronomically higher when you put yourself physically at the site of your target to try to break into a device. Additionally, nobody is going to let you walk up to a POS without asking questions -- you have to do it when nobody is paying attention, and even then there are hundreds of cameras in your average retail store. At least one is always watching each till.   

Besides all the risks to the attacker surrounding being caught red-handed, getting credit card data from them is remarkably difficult, as the data is tokenized at point of swipe or dip most of the time. At least, this is the case with most modern payment keypads. The terminal never actually sees the credit card data in a properly configured POS. However, an open terminal of any kind is still a tempting, potential target.

Realistically, there are better ways to go after a terminal, but let's imagine that is the attacker's goal...  

They compromise a Point-of-Sale system with the goal of infiltrating a store network and see how far they can go. In truth, if you follow the trail back to where the transaction requests go, they have to pass through a database that processes and logs the transactions for record keeping and tax purposes. There is _ALWAYS_ a paper trail for these transactions, and there is usually some way of either decrypting the tokenized data or reading it in plaintext somewhere along the line, usually at its final destination before it gets transmitted to the bank.

We have to assume an attacker needs to approach the environment carefully but casually, watching the store operations for some time in order to get a feel for how the staff operates the terminals, maybe attempt to get a hold of some credentials by watching them type their ID and passwords into the keypads. Then we can imagine for a moment a situation where a terminal is left unattended for just a minute. What can an attacker do in that single minute? This is our scenario.

## Preparation & Recon

If you are approaching this from a red-team perspective, you can assume an attacker is going to have plenty of time to casually wander the store and observe the employees as they work. Depending on the store layout, you can easily get a very good vantage point of a worker as they enter their credentials into their terminal while casually browsing the store. This is easy to do without looking conspicuous, so you can assume a determined attacker would do the same thing.

Most retail stores out there are expected to be using some kind of network segmentation to make sure that its payment terminals are separated from the internet. At the very least, POS systems should never have direct access to the internet, although you'd be surprised that it still happens. In any case, an attacker needs to have some way of ensuring they have a means of establishing persistence on a network if they are going to take a shot at compromising a terminal. They also need to know ahead of time some restrictions to tell them whether they can even hope to achieve any end-goals if they do manage to compromise one.

1. Does the terminal have a keyboard attached or _only_ a touch screen and/or keypad?  
2. Are there USB ports exposed? Can I attach a keyboard, or better yet, a rubber duck?
3. Do the terminals appear to have internet access? Is the desktop environment visible?
4. Are there network ports coming out of the terminal? Do they use Ethernet or serial? We must plan our payload accordingly.
5. Can a device be planted? Can I hide a leave-behind device somewhere with network access?
6. What operating system is this device running? Usually you can tell by observing the UI elements, but you can also reboot it when nobody is watching and watch what splash screen appears to figure it out.

Once we've established this information, we can decide how we want to go about making our approach.

In my case, the terminal I went after was a touch screen with no keyboard and a keypad that prevented the use of shortcuts and hotkeys. I knew ahead of time that it was a Windows machine, but considering it's popularity as an OS for business solutions, it should not be surprising to find embedded Windows in the majority of POS devices.

After all that, the only things I needed to create two plausible attack scenarios are the following pieces of equipment:

* A Raspberry Pi 3 with ...
  - 1 4G LTE card
  - 1 Panda Wireless PAU09 2.4+5Ghz dual-band Wireless NIC (supports packet injection & monitor mode)
  - A call-back configuration to our C2 server on boot so we can access the Pi
* A [Malduino](https://malduino.com/) Elite (or [Rubber Ducky](https://shop.hak5.org/products/usb-rubber-ducky-deluxe) if that's your preference)
* A small Network switch or hub, or a Hak5 [Throwing Star](https://shop.hak5.org/products/throwing-star-lan-tap) will do if you are only interested in passively sniffing traffic
* 2 3ft lengths of CAT6 cable
* A small, 3-outlet power strip
* A convincing-looking letter that says I have permission to be there

In all, it's approximately $140 of gear, plus any monthly costs for the 4G LTE plan. I leave it as an exercise to the reader to build their own Raspbery Pi leave-behind device. If you're not sure how to make one of these yourself, you can follow these instructions from my good friends from the SecureState War Room blog: [Create an Encrypted Leave-Behind Device](https://warroom.rsmus.com/encrypted-leave-behind/)

## The Approach

So in my case, I was _supposed_ to have permission to just begin testing the POS systems as part of the testing. I was operating under the assumption that the store managers were notified I would be arriving and that they knew what to expect.

Sadly... that was not the case.

![Hi I'm here to hack you](/images/posts/im-here-to-hackyou.jpg)

I walked into the store and introduced myself to the first cashier I saw, told them I was here to "test the cash registers" and that I needed to speak with the manager. The manager came down from the office and I explained my purpose for being there. They had no idea I was coming. They didn't know who I was. They didn't ask for identification. All I provided that gave some vague context to why I was there was a printed copy of an email. All it contained was the written word from my point of contact among the company security staff and a brief explanation to the store staff that they were supposed to cooperate with me. There were no signatures, just a phone number to my point of contact to call if there were questions. They called the point of contact and asked some basic questions of the person on the end of the line as to why I was expected. I want to emphasize that they did not verify the identity of the person they called either. They simply assumed the piece of paper I handed them was gospel, called the number on it, and let the person on the other end tell them what to do. Had this been a Red Team engagement, it could have just as easily been one of my team pretending to be someone high-up at the company. In the end, my point of contact simply told them to cooperate with me...

Well... they did. The store employees didn't so much as look at me twice after I walked over to the POS system and started plugging things in. From there I had a very solid vantage point of the store operations, and I had full access to a register all to myself. While I did have express permission to be at this store from the company leadership, I must emphasize that the store employees _did not know that_. They had no way of verifying my identity or purpose for being there. They simply accepted it and let me do as I pleased. To me, that's unacceptably scary, and it leaves me with no doubts in my mind about how someone could compromise a retail chain simply by walking in with a solid social engineering pretext.

## Dropping a Leave-Behind

The first step of my attack was assessing how I could get connected to the network on these same registers. As I had hoped, the registers were communicating over Ethernet with each other via a single port in the kiosk wall beneath the register. This is typically the case in more modern retail stores, as they are quickly getting away from the old SWIFT network systems that performed their payment card transactions directly through the payment processor. Everything is slowly moving towards SSL/TLS, which of course belongs in the family of TCP dependent protocols. If you are a bad guy, this leaves you with a very quick and simple way to gain your toe-hold onto the network. Simply replace the register's ethernet with a small ethernet cable connected to your ethernet switch or hub. A hub works as a great network tap as well as an opportunity to grab an IP for your leave-behind device. Finally, plug the register and your Pi into the switch/hub and you're off and running. Presumably, the register will be able to reacquire it's IP address. Depending on how secure the network is configured, you may have some trouble getting an IP address at first. In my case, the client had static IP addressing without MAC filters, so figuring out the IP address, netmask, and gatway address was the only hurdle.

At this point, you can simply walk away after about 20 seconds crouched behind the register plugging in all your hidden equipment and hopefully concealing it behind the big heavy register. In many cases, this is as brave as a physical attacker is going to get unless they know they can safely hang around and play with the register. Ex: If they have social engineered the staff into believing they belong there. Simply dressing in the same dress-code as the staff, wearing a polo shirt with the company logo on it, and a convincing looking stack of papers (clipboards work _great_ for this) is usually enough to fool the average person.

So what can we do if we have the opportunity to keep attacking the register directly?

## The Smash

While testing the registers, I was supposed to have access to the store functions, since this was supposed to be an authenticated, gray-box test. Unfortunately for me I did not receive credentials ahead of time, and I was not willing to wait for someone to find some for me to use. Instead, I inspected what I called the "home page" for the register where the employee login page was. On the front page before authentication, there are several functions available. The one that immediately got my attention was "Calculator" which simply popped up Calc.exe.

From Calc.exe I could open a windows help dialogue, which has a print function, which opens a print-to-file Windows Explorer window, which let me enter cmd.exe into the URI bar. Pop shell, pop in the Malduino, foothold established, game over.

Well, that was easy.

Essentially what this boils down to escaping the register application. As we discussed earlier, the register software is usually prefabricated or roll-your-own courtesy of the retailer's internal development team. Having applications with common escapes like this is all too common. Even assuming you can't pop calc to pop shell (oh the irony), there are usually plenty of other functions in the software for you to gain control of a shell or explorer window. The main thing to look for is print functions, since they tend to be everywhere. "Print Receipts," "Print Till Report," or similarly named functions are good places to start because you can bet there exists some way to get an explorer window open from a print dialog. Beyond that, there are lots of "escape keys" and shortcuts built into windows that the developers of the application may have overlooked and failed to disable. You can use these as ways to escape the application and gain control of a Windows OS application that eventually leads to a shell. I highly recommend this blog post from Dave Kennedy at TrustedSec on [Kiosk/POS Breakout Keys in Windows](https://www.trustedsec.com/2015/04/kioskpos-breakout-keys-in-windows/).

The best part of this attack was that it comes with so many more options. If you're working with a Malduino, and you know you can pull it off, the best thing you can have is a payload with Invoke-Mimikatz prepared so you can dump credentials right in front of you. As you no doubt noticed earlier, I mentioned that most of these POS devices are running as local admin, meaning getting local credentials is as easy as `sekurlsa::logonpasswords`. :sunglasses:

Honestly, if the security team at the retail store is doing things right, they won't allow the POS devices to have direct internet access, so getting a shell out to the internet C2 is going to be difficult, therefore credentials is the best thing you can get, especially if you have just successfully dropped a leave-behind on the network. Once you have that, it's time to leave and begin the work from our stealthy little hackbox.

## The Grab

So imagine we're back in our hotel, car, secluded hacker den, whatever suits your fancy. We've got our hackbox connected on our target network, and we have some valid credentials thanks to our earlier application escape and Malduino mimikatz payload. What do we do next?

Why, PSExec to victory, of course!

In my "unique" case, the register I popped had a locally configured administrator account, and even though they weren't on a domain, they all shared common admin credentials. This made controlling the registers remotely a simple matter of running the `exploit/windows/smb/psexec_psh` module to establish a foothold on all of these registers. The question is, how is this useful to us? What can we get from the registers? That is a question with a complicated answer.

More than likely, you'll find that all of the payment data is tokenized at the point of dip/swipe. This is a PCI recommended practice for POS systems that _thankfully_ is becoming very commonplace. It actually makes it quite difficult to steal anything valuable from the register. However, in certain cases, you may be able to find an encryption key that was used to tokenize the data before sending it to the payment processor. This is usually stored in a local MSSQL database which you can easily access via commandline, but if you're like me, you'll probably prefer a GUI to interact with the database instead. For this, I like to use [DBeaver](https://dbeaver.io/) which is a platform-agnostic SQL database client for virtually any database that supports SQL. That includes MySQL, MSSQL, NoSQL, MariaDB, MongoDB, and plenty of others. DBeaver also works with proxies and supports TLS encryption, so you can take advantage of port-forwards or a proxy port and secure SSL/TLS tunnels to make sure you're not exfiltrating sensitive data over an unsafe connection. We want to be responsible with our clients data when we steal it, after all :wink:

In my case, I was able to pull an AES symmetric key from the database which looked as though it was used for encrypting the data. Mind you, this key was identical across each kiosk. Decrypting the data wasn't part of the deal, so I simply documented the finding and moved along.

Next was to see how far I could take my access. Looking further in the file system, I took note of a folder labeled `C:\Scripts` containing a whole bunch of powershell files. Reading through them, I realized several of these scripts were run at the start of the machine's reboot cycle that, in essence, updated the device from some IP address in the local network that I had not seen yet. I ran Nmap against this system and came to find it was a Windows 2008 Server.

Reviewing the script, I also noted that _it was using the local Credential Manager to authenticate to the server_.

> Me: Please... no... It can't be that easy...

I ran PSexec with the same local admin credentials that were sitting on the registers and ... lo' and behold ... a system shell.

Unlike on the registers _this_ server was on the primary domain of the organization I was testing. Worse, a member of the "store administrators" group had recently logged in, and their credentials were still cached in Wdigest. Without even trying to, I had essentially owned the clients' domain reserved for their store servers from a single set of cash registers. The fact I could do this locally was bad enough, but knowing full well I could have done it with a throw-away hackbox connected to the same exactly place as my laptop, this could have easily been done from the parking lot.

## Fixing the Problems

For me, the assessment ended there. I continued pillaging the local network I was confined to, knowing full well I was able to easily carry my access into their corporate domain but not being allowed. The client was less than thrilled, and unfortunately they only removed the functions that allowed me to gain control of the POS terminal in the first place. The remaining issues of local shared admin credentials, shared decryption keys, horribly insecure authentication schemes, and hacky auto-patching scripts still exist as far as I know because they are "inherent to the design of the POS software."

The biggest fix I could have asked for out of this assessment would have been to remove local administrative privileges from the kiosk machines when running the POS software. It wasn't necessary except for the fact that the software developers _designed_ it to require admin credentials. Running a kiosk as a local admin is neither a new concept nor is it safe, and that has been known for well over a decade now. Secondary to that is disabling USB ports, _pushing_ updates from a server to the kiosk rather than the kiosk _pulling_ them from the server. Additionally using certificate-based authentication and machine trust, disabling shared local admin credentials, implementing [LAPS](https://itconnect.uw.edu/wares/msinf/ous/laps/), and enforcing static MAC address tables on switches to prevent unknown devices from being plugged into the network and pulling an address.

It would have also gone a long way to making me feel better about my visit if the staff had at least questioned my presence and reasons for being there a little more. It's a lot to expect retail store staff to be sentinels of security and knowing exactly what to look for, but if someone in a suit walks into your store and starts plugging things into your cash registers with no more reason than, "I'm supposed to be here", then you should _probably_ be at least a bit concerned. The fact I was not questioned beyond a generic piece of paper and a phone-call to an unfamiliar number brings a lot of further security concerns about how staff are trained when it comes to operational security.

Sadly, because none of those were in the scope of this assessment, the only thing that happened was removing a few poorly implemented features from the POS software. A step in the right direction, but definitely not out of the woods...

I would encourage you network admins who manage a retail environment or similar infrastructure to _really_ pay attention to how your POS devices talk to each other and what routes they provide in and out of your corporate network. At a minimum, you should pose the question, "What if someone compromises a cash register? What can they get and how far can they go?" Don't delude yourself into thinking "Nobody would be that bold," or "That couldn't happen to us." It can, and it probably will. These days, it's not a matter of _if_, it's _when_.

This was an interesting test that I felt needed to be shared, simply calling attention to the fact that there is some untapped potential here among kiosk devices as part of red teaming. Being able to demonstrate this kind of risk to companies who do 90% their business from behind a checkout counter is important not only to your clients but to the consumers they sell to. The real people at risk are the ones swiping their card, blindly trusting that that cash register they are transacting with is a safe one.

I hope you enjoyed the read and this will start sparking new ideas and angles for your Red Team engagements in the future. Thanks for reading! :+1:
