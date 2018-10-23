---
layout: post
title:  "My OSCP Journey Pt.1 - How NOT to take the OSCP"
date:   2018-10-23
featured-img: /images/posts/offsec-try-harder.png
categories:
- misc
- OSCP
---

I feel like this post has been long overdue given how long I've been chasing this dream of earning my OSCP. I officially started the course in February of 2017. So far, <u>I have taken and failed the exam a total of three times</u> despite approaching each attempt feeling profoundly confident that I would pass. Having proven myself wrong for the third time, I'm not going into this upcoming, fourth attempt with any assumptions anymore. I'm too cynical and exhausted by repeated failure for that. I've been humbled each time, so now I'm going to attack it with a different attitude. I'm completely starting over.

If there is one thing I've learned from the OSCP, it's how to embrace your failures and grow from them. Believe me, each time I screwed up was *NOT* an easy pill to swallow. Unfortunately, I don't see many other OSCP journeys that really talk about this... Maybe that's because most of them took it seriously enough that they didn't have to fail as hard as I did. Regardless of the reasons, I feel it's important to recognize when you failed and figure out how to recover, and I spent a lot of time failing. Given it's been such a long journey for me, probably longer than most, I thought it would be a wasted opportunity if I didn't share my experience with everyone else.  

So ... *-sigh-* here it goes.

# First Attempt

I started my OSCP journey on the dime of my former employer who was kind enough to give me a $1,200 spot to earn this highly respected certification that I'd been practically *drooling* over since I discovered it in high school (c. 2008). I went all out. Bought the courseware, 90 days of lab time, and got started burning the midnight oil trying to own every single box systematically that I could ... for about a week.

After the first seven days, I quickly got exhausted and let myself get lazy. I started focusing more on work, learning from the experiences of my new colleagues who were veteran penetration testers. This brought their own rewards, but it heavily distracted my from my pursuit of the OSCP. Taking online college courses at the same time didn't help either. Ultimately, I maybe exploited a total of 15 or fewer boxes in my first attempt before exam time. Given that I had bought 90 days, **this was a tremendous waste of time and money**.

Something else that I did wrong that I never corrected until recently is, _I made the mistake of not using the PWK VM image_. This critical error in my judgment, actively choosing to ignore the advice of the people who built the OSCP and PWK course, may have cost me more than the time I wasted by not spending it in the labs.

## **Exam 1**

Come exam day, I was thoroughly unprepared. I didn't schedule it until my lab time was about to expire, so this was approximately 1.5 months after my lab access ended. Let me emphasize that this was exceedingly stupid of me. That was 1.5 months I spent sitting on my ass, stressing about an exam I could no longer adequately prepare myself for without buying more lab time with money I did not have.

Second mistake, I scheduled it starting at 5PM local time. I had been up for half the day already, already caffeinated with two cups of coffee, and entirely too overconfident in myself.

### First box down
By the first 2 hours I successfully took down my buffer overflow box despite issues connecting to the VPN. I was feeling very good about this, and thought I had it in the bag. In reality, this was an easy 25 points I was **expected** to get easily. The buffer overflow, while a difficult concept to grasp initially, is probably one of the easiest boxes to exploit once you know how to do it. Abusing a stack overflow flaw, once learned, is like riding a bike. Just in case you forget or haven't learned yet though, here's a link to the [Corelan Blog](https://www.corelan.be/index.php/2009/07/19/exploit-writing-tutorial-part-1-stack-based-overflows/), recommended to me by [@ZeroSteiner](https://zerosteiner.com/).

Despite feeling pretty good about my first (easy) 25 points, this would be the end of the easy for me. From here, the points came slow and painful.

### Eight hours in ... starting to lose confidence
Six hours since my last set of points being earned, I was beginning to stress about the outcome. I realized I had not nearly prepared myself with knowledge on service enumeration at this point, and I was running the same tools over and over expecting some different magic to happen.

When suddenly *-gasp-* ... another shell!

After digging through a static website on the box I was targeting, I came across some information that led me to start extending my port scanning a little bit. I managed to come across an obscure service running on an obscure port with an *exceedingly* well documented exploit. Despite it being 10 hours after my exam began and already feeling slightly defeated, this put some wind in my sails. It was the confidence boost I needed to continue. Unfortunately, it would be short-lived.

After getting my limited shell ... I promptly lost it by running a command that would never end. I tried re-exploiting to no avail because I had locked the thread I was in, and there was no getting it back without restarting the running service. I would be forced to spend a revert.

After three more reverts because of repeatedly making the same stupid mistakes, I realized I was getting no where. I burned my one-time Metasploit token to get a more stable shell, but even that did very little to help. For whatever reason, meterpreter would not work. I wasted an hour trying to make meterpreter work because I thought it would save me somehow. When that failed, I fell back to a more stable but featureless perl shell.

I found myself uploading useless tools that offered me no help, because I had not worked hard enough learning privilege escalation that otherwise should have been easy. After another three hours wasted and only a local.txt proof file to show for it, I dejectedly went to attack my 10-point box.

### Burn-out
I am at 16 hours awake. I've guzzled two Monster Rehab energy drinks, and three cups of coffee at this point. No sleep, and only one break for a cheeseburger from the local fast food joint. The 10 point box has yet to yield itself to me, and I am starting to feel the pressure of the clock on me.

I'd spent hours chasing a vulnerability on the 25-point box that, although it was the correct one, I did not know how to think for myself enough to modify my exploit and leverage a shell. I walk away from the 25-point box with only 6 hours left in my exam. My eyes burn, my fingers feel numb, and my head is in agony, begging for me to pop 1000mg of Ibuprofen and go to bed while my hands shake from the absolute overdose of caffeine I was experiencing.

I tried and failed to sleep for more than an hour, lying wide-eyed awake and literally vibrating from the level of caffeine in my system. I give up on trying to sleep and drag myself back to my computer. Things were *not* going according to plan...

### 20 hours ... Panic sets in

At this point I know I am going to fail. I have less than 4 hours and have not seen a shell since my last session on the 20-point box (which is still alive). I bounced around every machine, including my local shell, retrying things that I thought I could modify before quickly giving up and going to another machine.

I am looking ... begging ... for one of these systems to give me a shell, but it never comes.

I look up at the clock after staring at a blank terminal for almost 30 minutes in a foggy haze and see that I have less than 20 minutes before my lab access ends. I decided that was the end, and I couldn't go any further. I crawled into my bed a shell of the man I was, too exhausted to think or come to terms with my failure.

> "It's over... I didn't try hard enough..."

I started writing my report 20 hours later after spending 14 of them sleeping. I knew full well it would be pointless. I had not created a lab report. I didn't even collect any evidence. I was so certain it wouldn't be needed. In truth, it wasn't, because it wouldn't make a difference. I failed hard. After turning in my exam report, I crawled back into bed for another day.

### Exam 1 Scoreboard

|-----+--------+--------|
| Box | Access | Points |  
|-----------------|:---------------:|-----------------:|  
| Buffer Overflow | Root            | 25 points        |  
| 25-point box    | None            | 0 points         |  
| 20-point box 1  | Local           | ~10 points?      |  
| 20-point box 2  | None            | 0 points         |  
| 10-point box    | None            | 0 points         |  
| Lab Report      | None            | 0 Points         |
|-----------------+-----------------+-----------------:|
| **Final Score** |       ---       | **~ 35 Points**  |
| **Status**      |       ---       | **Annihilated**  |
| **Judgement**   |       ---       | **You suck ... Try Harder** |  

I have never cried myself to sleep. But there's a first for everything I suppose...  

The OSCP ate me alive like a predator to their prey, and I never felt lower or more humiliated for ever feeling confident in my skills as a hacker. As far as I knew, I had none. If you are reading this after failing, trust me when I say I know *exactly* how you feel... along with 60%+ of exam takers who also failed at least once.

I spent well over a week recovering physically and mentally ... and emotionally before I was ready to re-assess what went so horribly wrong...

## **Everything that went so horribly wrong**...

I told you at the beginning that this would be about embracing failure, so I'm going to be up front and lay all of mine on the table for you. Ultimately, __I made seven major errors__ that led to my inevitable failure during my first attempt. If you can avoid making them yourself, you will be better prepared.

### **1. I ignored the PWK course's No.1 recommendation**  
> **"USE THE VM WE GAVE YOU!""**  
 --  OSCP devs   

  The Pentesting with Kali VM is there for a reason. It says it right there in the description *it is built specifically for the lab environment and the exam.* This is for many reasons, but chiefly among them, the VM is built with an i686 architecture. If that doesn't strike you as weird, it should. Much of the lab and certain exam machines depend on using compiled exploits that you will need to be able to figure out how to compile on your own. I will emphasize *compiling exploits is f0rkin' hard* if you don't know anything about C, compiled languages, or the gcc compiler and it's near-infinite number of flags.

  Because the exploits you'll use in both the lab and exam will almost certainly fail if not compiled correctly, the all-knowing OSCP overseers graced us with a VM that comes with everything we need. __The PWK VM is built to compile all exploits correctly__ with all the necessary libraries already included. Worst-case scenario, you may run into some compilation errors that can be fixed with minor tweaks to the exploits, adjusting some shellcode, or adjusting some well-documented compiler flags. In most almost every case, the exploit should compile without any flags at all, otherwise you will find it with some prime-choice googling.

  The course says, *"Use a custom image at your own risk."*  
  I say, *"PWK VM or bust!"*

  In addition, don't update the VM either.

  `apt-get dist-upgrade` *is a dangerous gamble!*

  I later discovered that several tools in the VM will break after updating them thanks to some drastic changes that have been done to the main Kali Rolling disto that will not translate correctly in the PWK VM. It was built the way it was for a good reason. Use it and don't change it. You may unknowingly break something that was intended to help you pass the exam.  

  Don't try to be a smart guy like me. Get the PWK VM, step outside your comfort zone, resist any temptation to screw with it, and use it as it was intended. It may make the difference between success and failure. You don't need the newest, shiniest updates.

  Just tell yourself... ![](/images/posts/idontneedit.jpg){:width="200px"}

### 2. **I did not plan enough downtime to dedicate to the course.**
  This is probably the primary cardinal sin of all who attempt the OSCP and failed. Life often gets in the way, but for a lot of us, we allow ourselves to get lazy or distracted from our goal. The OSCP is no joke, and if you allow yourself to get overconfident, think you've got it in the bag, and don't give it the amount of study it deserves, you will inevitably lose.

  - At a minimum, I should have been spending 4 hours per day in the labs. Instead, I maybe did a handful of hours per week on average.
  - If you are in school like I was, put it on hiatus or wait until you're finished. You just can't do both...
  - If you are working a full time job like I was, you need to make sacrifices. No games, no playtime. There are only the labs. That's your life now until your exam day.  

### 3. **I did not spend enough time learning how to enumerate and escalate.**
  This may seem like an obvious one, but learn the tools they've given you. Many of the machines in the labs and the exam force you to enumerate *several* services. If you spend all of your time digging for an easy 1-2-3 RCE exploit, you're only going to hit maybe 10% of the labs, and you will definitely fail your exam.

  Worse yet, most of the exam boxes will require you to escalate your privileges in some way. In the real world, this can be just as difficult or more-so than what you experience in the OSCP exam. You *must* be capable of using tools that pale in comparison to the functionality of Metasploit and other escalation tools and still be equally successful.

  Learn how to produce [shells](https://highon.coffee/blog/reverse-shell-cheat-sheet/), [upload/download](https://4sysops.com/archives/use-powershell-to-download-a-file-with-http-https-and-ftp/) files, and [enumerate](https://highon.coffee/blog/penetration-testing-tools-cheat-sheet/#enumeration--attacking-network-services) users, groups, services, and processes with native OS command line. This includes [Windows](http://www.fuzzysecurity.com/tutorials/16.html) and [Linux](https://payatu.com/guide-linux-privilege-escalation/). If you're a Linux guru, half the work is done for you, but Windows Escalation proved to be my worst nightmare. Tools like wget, curl, netcat ... those don't exist in the Windows world.

  I strongly advise learning the Linux CLI until you're a champ at it. Make it a daily exercise to learn a new tool and learn to read the *man pages*. For Windows, keep the [**Windows SysInternals**](https://docs.microsoft.com/en-us/sysinternals/) tools close at hand, particularly ...
   - AccessChk.exe
   - ShareEnum
   - ProcDump
   - PsExec
   - LogonSessions

   In addition, any of the major things you do in Linux escalation, you should learn how to do from Windows command line, like file & string grepping, listing processes, string concatenation, echoing streams to files, listing users, groups, shares, permissions and running services/processes. All of that comes into play come exam time.

### 4. **I tried to be an Army of One**
  Instead of looking to other people for advice, going to the OSCP forums, or asking questions in the IRC, I tried to be the cool kid who could figure it out all by his bad-ass self. Well, I didn't, and I failed miserably in my first attempt because I was too cool for school. Don't pretend you'll get it on your own, because if you learn from your predecessor's mistakes, you can avoid making them yourself and save yourself a lot of heartache (and money.)

  Take the time to set yourself up in the OSCP's official forums, IRC, and the [NetSec Focus OSCP Slack channel](https://netsecfocus.herokuapp.com/). The information there is invaluable, and you can always ask questions (or look at the questions of others) when you eventually get stuck. Sometimes just talking to someone about it will kickstart something in your mind that leads you in the right direction. I can speak from experience that nobody wants to spoil it for you, so don't expect people to just give you the answer. You should neither be nor expect to be spoon-fed. That's now how you get good at hacking. As Yoda put it ...
  > Do, or do not. There is no "Try."

### 5. **I attacked the network like a to-do list, not as a pentester**

  I made this mistake repeatedly throughout the course, but the lab is a wide open network meant to simulate a realistic environment. Even though you'll probably start by listing targets according to their IP address, don't be tempted to attack them in that order.

  The environment offers a lot of machines with varying challenges and opportunities to learn new ways of exploiting system flaws. Your time spent going after them should be organic in that you attack things that catch your interest or require you to take a different approach than you are accustomed to. This is part of how the OSCP makes you evolve as a pentester, so you should let it happen that way.

  A few extra tidbits for attacking the lab as to best prepare for the exam...

  - Don't rely on automated scanning tools. Nessus and OpenVAS are great, but they will give you tunnel vision on many of the hosts you attack. It's also bad form to implicitly trust scanners to deliver you low-hanging fruit, because you will never force yourself to try and find things that the scanner can't. This will cripple you for the exam AND as a penetration tester.
  - Force yourself to put Metasploit away. You can use it the first couple of times you exploit something, but ultimately you aren't going to have it in the exams. Once you successfully root a box, repeat the process using tools outside of Metasploit. Use the Nmap NSE scripts, as well as other enumeration tools to enumerate services, and learn how to compile, upload, deploy, and execute exploits without Metasploit.  


  You still need msfvenom to generate shellcode, but the majority of the framework will be off limits to you, so learn how to survive without it. When your box of survival gear gets taken away during the exam, all you're going to be left with are your rocks and sticks, so you'll need to learn to craft your own tools in the wild anyway. Best to be prepared.

### 6. **I did not schedule in advance**

  By the time I scheduled my exam, my lab access was coming to an end. Like a moron, I thought I could just schedule it for the following week no problem. As it turns out, the exams for OSCP are typically booked for upwards of a month from the current date, so you **MUST** schedule it in advance.

  There is nothing worse than spending months in the labs and then having to wait another month just to take a crack at the exam. Schedule ahead so that you take an exam attempt almost immediately after the labs, around 1 week is usually sufficient. This will give you a chance to script up some useful tools and mentally prepare yourself for exam day.

  An extra added piece of advice... take the exam whether you think you're ready or not. You have nothing to loose because you'll need to purchase more lab time, which comes with a complementary exam attempt.  

  Don't waste the chance and go for the exam, even if you don't feel ready. Taking the exam is the best way to prepare for it, and although it's not impossible to pass the first time, the odds indeed are against you.

### 7. **I was over-caffeinated, over-confident, and completely exhausted**

  This mistake will often lead even good students to snatch defeat from the jaws of victory. The OSCP is a monster of an exam, and you will likely spend almost every hour trying to get the minimum 70 points unless you prepared like a champion. Like many others, I dug my own grave by
  1. Scheduling my exam late in the day (any time after 12pm is probably a bad idea)
  2. Not sleeping well the night before (mostly because I was too excited)
  3. Over-caffeination destroyed my ability to think clearly and left me not only physically but mentally exhausted.
  4. Not taking breaks during the exam left me with tunnel-vision almost the entire time. I focused on 1 vulnerability that I was **certain** would get me shell that ultimately was just a red herring.
  5. Not sleeping, which ... again ... over-caffeination made life much harder. I tried, and was literally so jittery I could not force myself to sleep, so I forced myself to continue with no energy left in me. My fate was sealed at that point.


## Wrap-up
Avoiding these mistakes will hopefully keep your head above water while taking the OSCP exam, but ultimately, the biggest key factor is your preparation. Spending ample time in the labs is essential to pass. If you are considering taking the OSCP, you should at a minimum take the 90-day course to start and add on as necessary. Take the exam as often as possible and don't miss any opportunities, even if failure is a certainty, you have nothing to lose anyway, so you might as well take it and learn from it. Do your best.

My OSCP journey still isn't over, so I have more to share. Seeing how long this post has gotten, it's worth a multi-part series and probably a lot more than that. I'll let this first part stand as a **How not to take the OSCP** because that's exactly what it is. In the next part, I'll cover how I switched things up and made some of my improvements for my second and third attempts at the exam.

Thanks for reading and if you're feeling like I did in that moment of defeat, just know that most of us have already been there. There will be another day, and you'll do better next time. You just have to *Try Harder*. :)
