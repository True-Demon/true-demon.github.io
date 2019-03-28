---
layout: "single"
title: "OSCP Journey Pt.3 - Preparation Tips and Avoiding Mistakes"
date: "2019-03-28 12:24"
author_profile: true
toc: true
---

{% include figure image_path="/images/posts/panic-button.jpg" type=center %}

If I were to summarize what leads to success on the OSCP in a single word, it would be **persistence**. With each attempt I made at the exam, things got a little easier. For whatever reason, my way of passing the OSCP came through brute-force, repeated iteration, a _lot_ of screaming into the pillow, and absolute brick-like stubbornness. I understand that most people shouldn't _need_ as many attempts as I have but... here I am -- still trying.

Maybe my OSCP journey is less about being a bad-ass hackerman and more about learning how not to give up so that I can pass on the knowledge. Whatever the case may be, this next post is to cover some of the essential information I think everyone should have when they prepare to take the OSCP.

## Collecting Tools & Trade Secrets

The OSCP is less about being a genius hacker than it is about being a stickler for methodical enumeration. You might get the impression that the OSCP requires you to be insanely knowledgeable about all things computing. Spoiler alert -- it doesn't. What the OSCP really wants from you is to understand how to be thorough. Real penetration testers spend more time reconnoitering an environment than actually attacking it, unless you get lucky with an obvious RCE or you cheat and use a vulnerability scanner to give you the answers, but that's no fun, hence why it is banned for the exam. Getting a feel for what tools you should start with, and how you should approach a given network or system is an important step in the learning process that I had ignored in my previous attempt.

How I rectified this was by completely re-reading and keeping an open copy of the PWK Guide open on my computer at all times so that I could reference it as I stepped through the labs. I stopped trying to hit everything at once and instead begin developing a set of tools and sequential steps for enumerating services as I went along. Time and time again, I found that **nmap** of all things was what I needed 90% of the time because of the nifty little NSE scripts it has. This wasn't the only tool I could make use of, naturally, but if someone had a tool to enumerate a specific service, someone probably already made a handy little lua script for the Nmap Scanning Engine already. Sure enough, I found that developing my own scripts for scanning services was less necessary than just getting REALLY good at remembering to check the `/usr/share/nmap/scripts/` directory for something related to what I was scanning.

The second thing I learned to do was to **bookmark absolutely everything**. Having a dedicated folder in your bookmarks tab for tutorials and guides might seem like an obvious thing to do from the beginning, but I'm probably one of the dumber students the PWK course has ever seen, so I didn't think of it before hand, and instead relied on my fire-from-the-hip Google-fu to get me through. I found that the OSCP forums were great for getting hints and tips on how to exploit specific boxes, but not so much at finding good research material. Instead, I started looking for _other_ "how not to bomb the OSCP" guides and started collecting their reference material. In particular, I got an enormous amount of information from a few golden sources.

* [How-to-OSCP](https://gist.github.com/unfo/5ddc85671dcf39f877aaf5dce105fac3)
* [OSCP-Prep Resources](https://github.com/burntmybagel/OSCP-Prep/blob/master/README.md)
* [Teck_k2 OSCP Guide](https://teckk2.github.io/category/OSCP.html)
* [James Hall's OSCP Preparation](https://411hall.github.io/OSCP-Preparation/)

It's through these few resources that the flood-gates opened to link after link of new tools, resources, handy tips, and things I needed to know to even stand a chance. I particularly got a great deal of information from James (411) Hall when he pointed out useful enumeration scripts and automating a lot of the manual digging around for privilege escalation information, but we'll get to that later.

Besides gathering tons of links on "how to OSCP better", I started looking a lot more closely at the scripts and tools in the Kali applications menu. It's funny how much you can learn by simply reading the MAN pages for a change.

## Expect the unexpected

A lot of very interesting things can be encountered in the OSCP labs that you just don't count on or have never seen before. Really what this is doing is getting you in the habit of exploring things outside of your comfort zone. I failed to do this in my first attempt at the exam, but this changed immediately after reassessing my approach. Rather than looking over the whole network for the low-hanging fruit, I would essentially roll the dice and pick a system at random to start looking into. When you look at the OSCP labs like a network administrator, you can easily see that there is absolutely no congruence, no obvious relationship between any of the systems in the network other than the fact that they all have something wrong with them.

There are a small, select few systems that share some relationship that causes one to lead to exploiting the other, but the majority are their own puzzle to solve. It leads to a lot of frustration and hair-pulling, but finally ends with ultimate gratification once you figure out the answer. Often times, you'll find that the answer was very simple, and that eureka moment was something like a carrot on a stick for me. It motivated me just enough to endure going after the hardest boxes for the sake of learning something new.

When you look at the labs this way, especially if you have done penetration testing for enterprise organizations, you can allow yourself to throw out the book on what you know works, because it won't work in the OSCP labs. Things like Mimikatz, dumping hashes, sniffing credentials off the network, those things aren't _usually_ going to be useful to you. It's too easy, and you wouldn't learn anything new if that's all it took. Again, there are exceptions, but try to exhaust all other possibilities before looking for that "link" between boxes.

I also made it a habit to organize each box and document or pipe the output of every single thing I did to any given system. Re-running scans is an exceedingly annoying and obnoxious procedure that you can avoid simply by adding the `-oA` flag to your Nmap scans. They can be exceptionally useful to you in ways you didn't expect.

For instance, did you know that **searchsploit accepts nmap.xml files as input for searching exploits**? I didn't -- until now that is, and like everything else I learned in the OSCP, it shocked me with how easy it was. Chances are, if you've encountered something you've never seen before, someone else has, and they probably have a tool for that.

## Arming up

I've hinted at a lot of tools that made my life infinitely easier during the labs. There are probably too many to actually list out or this post would turn into an encyclopedia, but I'll highlight some of the real life-savers.

1. **Nmap** obviously goes up to the Number 1 spot here, but specifically, I wanted to highlight some of the syntax voodoo that I found made life easier for me.
  - **Always use the `-n` option to prevent DNS resolution**, which wastes unnecessary time, especially during the exam. The hosts will never resolve anyway, which causes the scan to delay about 30 seconds per host.  <br>
  - **Always use the `-oA <filename>` options to keep a set of files**. The .xml file can be imported to other tools like the MSFconsole database so you can review them quickly, and searchploit using the `--nmap` option to identify possible exploits based on scan data.<br>
  - **Always use the `timeout` options on large scans**. I don't know if I'm the only one who experienced these issues, but nmap scanning became a grueling slog when trying to scan large spans of ports. Preventing Nmap from exponentially increasing scan time by manually setting probe timouts, maximum retries, and minimum scanning rate via `--max-rtt-timeout`, `--max-retries`, and `--min-rate` is important for guaranteeing your port-scans actually finish in a reasonable amount of time.  <br>
  - `-sS` before you `-sV`. Syn scanning requires drastically less time than version/fingerprint scanning. For some reason, Nmap decides that it will repeat the `-sV` on all possible ports, even ones that the `-sS` scan has determined are closed. By performing the `-sS` scan and using the -p option to target specific ports for your fingerprint scan, you save a _lot_ of time.  <br>
  - `nmap -sS -p21 --script ftp-\*` Using the `\*` can parse through the /usr/share/nmap/scripts folder for any nse script that starts with "ftp-", crawling through the ftp scripts, including one that searches for anonymous logins, something that was very common in the labs and somewhat time-consuming to check manually. You can apply this to virtually any other protocol like `smb-\*`.  <br>
  - `nmap -sS --script \*vuln\*` Scan for known vulnerabilities. Almost all of the vuln-scanning scripts have the "vuln" keyword in the title, and can save some time. "cve" is also common as well. If you don't want to waste time, be sure to isolate ports and protocols first.  <br>
  - Can't find any interesting ports? Don't forget `-sU`. The UDP scan is often overlooked during the exam, but it's a critical one. Especially if you find an SNMP service running. SNMP is a huge opportunity for host enumeration, and it's an easy test to check for SNMP on port 161.  <br>
  - Be selective about your ports via `--top-ports` and `-p1-10000 -r`. Sometimes the most popular ports are what you're looking for, sometimes they aren't. The OSCP labs are good about trolling their students, so you'll be surprised to find random services running on weird port numbers. You can save yourself some frustration by being selective about your portscans and using the `-r` option to force sequential port scans, rather than based on their likelihood ratio.  <br>
  - Read the [NMAP docs](https://nmap.org/book/) for more awesome tips. It's useful stuff.  <br>
2. **Enum4linux** is a fantastic tool for enumerating SMB and NetBIOS ports, which are notorious for being chock full of information disclosure bugs and misconfigurations, namely when null-sessions are allowed. When I see SMB and NetBIOS open, this is the first tool I run, and sometimes it's all I need.  <br>
3. **smtp-user-enum** and **ssh-user-enum** are two exceptionally useful and fast-running tools that can quickly validate the existence of users on most linux hosts. Combined the metasploit unix-users wordlist, you can quickly confirm common users that might be useful for logging in. During the OSCP labs, I noted that sometimes the common user isn't the end-goal though, so you will probably need to study the box a bit and find out the names of likely users. Here is a copy of the latest OpenSSH User enumeration flaw, if you don't already have it. [OpenSSH 2.3 < 77 - User Enumeration](https://www.exploit-db.com/exploits/45233)  <br>
4. **Gobuster** has been my preferred alternative since DirBuster support was pulled. It's built into the repositories for Kali, but must be installed via `apt-get install gobuster`. It's easily my favorite directory brute-forcer for its speed and multi-thread capability, and an absolute must for the OSCP course.  <br>
4. **[beroot](https://github.com/AlessandroZ/BeRoot)** is extremely handy for enumerating common privilege escalations in windows. It's a bit dated, but still an excellent go-to for quickly eliminating the most common exploits as a possible path to NT System.  <br>
5. **[JAWS](https://github.com/411Hall/JAWS)** is also a solid powershell alternative for privilege escalation enumeration from James @411Hall Hall that was equally powerful as BeRoot.  

## Prepare for Shell Hell

Your shells will get dropped _constantly_ during the labs, and you're expected to know how to react to that quickly. Figuring out ways to quickly obtain a shell _and_ make sure it is stable will be a brutal frustration during your time in the labs, and especially while you are racing the clock during your exam. For that reason, I **strongly** recommend becoming an absolute wizard at figuring out multiple ways to deliver, execute, and stabilize your shells.

`msfvenom` will be your BFF for the duration of the labs, but I strongly recommend you do not get too comfortable with meterpreter. You will not have the luxury of using meterpreter more than once, and in my experience, most of the modules have not been helpful. You're better off learning the hard way during the labs and avoid the heart-ache and absolute panic you'll experience when you realize you have no idea how to manage a raw TCP shell.

Focus instead on working on being able to get raw shells via **netcat**, **socat**, **powershell**, **python**, **perl** and other native execution methods. [Arr0way's Reverse Shell Cheat Sheet](https://highon.coffee/blog/reverse-shell-cheat-sheet/) is an excellent tab to keep open during the labs in case you run into something you have not encountered before and are desperate for more shells.

The main thing is to train yourself to be able to operate independently of Metasploit and the meterpreter shell. It is powerful but highly detectable, and altogether unavailable to you during the OSCP exam except for one system. My strategy was to find and execute local exploits the first time with meterpreter, then go back and repeat the process using a raw TCP shell and native tools, including the enumeration process. Once you've done this the first few times, try exploiting a system entirely with the raw shell and don't rely on metasploit at all. This will help prepare you for the kinds of activities you'll need to perform.

Knowing how to port-forward with plink and ssh, having multiple methods for downloading and uploading files to your target, and a cheat-sheet of enumeration and common commands will keep you on-track and avoid wasting precious time researching how to perform common enumeration tasks.

## Plan your attacks with a To-Do list

One frequent problem test-takers encounter during the OSCP is that they don't plan ahead and will clumsily hammer away at hosts using different tools almost at random. You may get lucky sometimes, and you might not. During the OSCP labs, you can't afford to miss anything, and you never know where you will encounter clues or hidden services to allow you to exploit a system.

What helped me a lot was to create a script for me to follow when enumerating a new box. Not an automated script, mind you, but a list of tools and common things to check for whenever I set about attacking a fresh box. This will help you with your note-taking process and ensures you don't forget to run all the tools necessary to fully enumerate a system.

## Be persistent, take your breaks, and know when to start over

Tunnel vision is a big killer on the OSCP exam, and a frustrating learning experience during the PWK course. Getting too focused on one thing for too long can eventually lead to endless rabbit holes and wastes previous time. Finding the balance between persistent determination and knowing when to pull back and try something else is perhaps the hardest lesson to grasp.

I've learned through the PWK course that not everything will be straight-forward, whether it's the labs or the real thing. Automation will only take you so far, so developing your intuition for finding chinks in the armor of any system is perhaps the best learning experience you'll get from the labs.

My rule of thumb is, when you find yourself repeating the same steps and running the same tools on a specific port, website, or whatever, that's when you need to stand up, walk away, and take a break. When you eventually return with a fresh perspective is when you think to yourself, "Maybe there's something I missed..."

Run a different port scan, run it a different way, start over with your enumeration completely, start looking elsewhere. I've found that when you feel absolutely stuck because "there is nothing there", that is the time when you need to step back and start over fresh.

## Screenshot and document as you go

Reporting is absolutely _everything_ when it comes to the OSCP exam. It makes sense, seeing as it's ultimately what your client pays for when you perform a penetration test. Obviously it doesn't matter if you can own a network six ways to Sunday if you can't communicate that to the client in a meaningful way, and that is the kind of emphasis on reporting that makes the OSCP so unique.

That being said, it's important that you don't forget to document every detail, about every box. It's best to do these things as you go along your way during the exam. Each time you find a new clue, service, flaw, exploit, and flag, take a screenshot and drop it into a folder, preferably one backed up to the cloud. Given the number of reboots and technical issues I had, I can tell you that it would have been made far worse had I not been maintaining all of my notes and screenshots in a cloud-backup folder that synced automatically.

Another important note on reporting, when you first get your VPN access, the first thing you should do is go to the OSCP Exam Panel, the page where you submit your flags, and save a copy of the system descriptions and reporting instructions. It will tell you how many screenshots and the level of detail expected of you in your report for each individual system.

Obviously document your flags, and submit them immediately. You won't get *any* points if you don't submit your flags in the Exam Panel, even if they show up in your report later. I'd also recommend reviewing the [OSCP Reporting Guidelines](https://support.offensive-security.com/oscp-exam-guide/) before your exam so you have a fresh idea of what to expect.

## Have a backup system

Arguably the most important advice I can give is to have something to fall back on if your system fails for any reason. My primary setup was a laptop with Fedora 29 installed and using VMware Workstation.

I ran into a bunch of troubles that could have been bypassed during several exam attempts if I had just switched to a different system.

My advice is to simply make sure you've got a backup plan for if your power or internet goes out, and if you lose for machine or current VM. I later prepared by cloning my PWK image to a second computer, but you could also have a NAS or even a really fast USB 3.0+ flash drive or external hard drive. As long as you have at least two options, you can give yourself a little extra insurance.
