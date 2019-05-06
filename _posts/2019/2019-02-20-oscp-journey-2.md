---
layout: "single"
title: "OSCP Journey Pt.2 - Close but no Cigar"
author_profile: true
toc: true
date: "2019-02-20 11:33"
---

{% include figure image_path="/images/posts/close-but-no.jpeg" type=center %}

It's been a while since my last post, having been working hard on my latest attempt at the OSCP and juggling work and college courses in the meanwhile. I thought that as my next and _hopefully_ last OSCP attempt approaches, that I might go over some of the additional things I've learned through my constant studying. If you didn't see my post about my first attempt, you can check it out [here](../2018/2018-10-23-OSCP-Journey-Part-1). I highly recommend it if you're looking for some tips about things to avoid and the pitfalls I encountered while studying.

The next three attempts at the OSCP were frustrating busts as well, but each attempt saw me getting closer and closer to passing. This time, I'll cover some of the things I did right, but fell short anyway and why.

## Prep-Time

The second attempt at the exam, I decided to go for an additional 30 days of lab time, mostly because I was short on cash and at this point, I was obligated to pay on my own. Unlike before, I was hell-bent on spending several hours a day in the labs in order to prepare myself. Preparing for the OSCP, as I have said before, is something you have to dedicate enough time to in order to consider it a part-time job. It's easier when you don't have a job, school, and family to worry about all at the same time, which is why I advise newbies to go straight for the OSCP as soon as they have several months where they don't know where to go next. In all, I spent about 3 hours a day on average, 5 days a week. Still slim compared to the amount of time one should be spending in the labs, but it was still a dramatic improvement over my last attempt.

One of the things I did immediately once I got the chance was I scheduled my exam as soon as I was able to. With just 30 days in the lab, I knew I needed to schedule right away so that I would be taking the exam as soon as my lab access ended. In truth, the earliest I was able to schedule on a weekend was three weeks after the lab ended. Not ideal, but better than waiting over a month. With that, study time began.

## Try, Try Again ... and again

I subsequently attempted the OSCP three more times in the year following my first failure and still had not passed. Although I cannot comment precisely on the boxes themselves or the services I encountered, I think I can safely talk about some of the frustrating obstacles.

During each of these attempts at the exam were my inability to step back from my tunnel vision and my failure to properly enumerate all parts of a machine to the point where I knew everything about them. I had a healthy mix of Windows and Linux systems between both exams, but one box in particular which I got on both was particularly frustrating and nigh impenetrable for me. Perhaps my biggest mistake was **over-thinking** the boxes.

It's true what they say that the OSCP does not demand you be a genius. Nothing outlandish is expected of you. You are expected to do your own research, but the concepts covered in the courseware and labs essentially cover all of the fundamentals. If you have blitzed through the labs, all the way through to the Admin network, then owning the exam should be no sweat for you. Look for the obvious things, and you're more likely to succeed.

There's no need to over-think it. Unfortunately, I didn't learn this lesson quickly enough.

### The Proctored Exam

I had the rare experience of doing both the unproctored and proctored exams. My fourth exam attempt was a proctored one. Although I would say that the proctoring did not add any difficulty or pressure to the exam, it wasn't without its issues.

At the time of writing, the proctoring consists of a webcam session (no audio) and a screen-sharing session so that the proctor can view all of your monitors. The main purpose

My biggest problem with the proctoring was the screen-sharing software OffSec uses, ScreenConnect, which is a product of ConnectWise, a product I was familiar with as a network administrator. When used with Linux, it uses a java binary to connect you to the session. Unfortunately, this product directly hooks the xorg server in a way that leads to problems. I found out through repeatedly encountering issues with ScreenConnect that it is extremely buggy if your host machine is a Linux distro with an X-Desktop environment such as GNOME.

If the application closes, it will cause the xorg server to hang because it fails to detach itself properly, resulting in a frozen screen that you cannot recover from. The only option is a hard reboot. This isn't OffSec's fault obviously, but it did cause massive problems, especially due to the fact that ScreenConnect itself slowly bleeds memory over time, forcing you to eventually reset at some point.

I was not able to test this with Windows, but in my past experience with the ScreenConnect software, I had not encountered a bug like this. I did report the issues I experienced to OffSec in hopes they will engage their vendors to fix the bug. Hopefully the problem is fixed for future exam takers, but be aware that if you are using a Linux host, there could be problems.

### Network Problems & Technical Difficulties

During each exam, I encountered network issues with the VPN, mostly with latency while port scanning. The connections don't seem to be able to handle very much traffic simultaneously, so I learned quickly that you need to be very slow and methodical about your port scans, otherwise you're likely to miss several things. It's also quite frustrating to bang your head against the wall for hours only to find that the actual exploitable service was hidden on a different port number that your previous scans had missed.

My fourth exam attempt was perhaps the most disastrous because of how close I came to success, but technical problems sealed my fate. It was my first proctored attempt at the exam, and it began with some network problems. The VPN wasn't behaving properly due to how I had connected.

**Pro-Tip: Always start your VPN from the Kali VM in a terminal. Don't configure it through GNOME or your host machine.**

I wasted more than two hours because of less-than-obvious networking problems. Then, half-way through the exam, my laptop experienced a display driver failure because of DisplayLink, causing my monitors to fail. During multiple reboots in my attempts to fix it, my (POS Dell) laptop decided it would be a fantastic time to upgrade the BIOS for the next hour. Following that, half of my kernel modules did not work anymore, and I spent altogether a total of 6 hours trying to fix my laptop because I didn't have a backup.

With that said, **I strongly advise having a backup computer!** You'll suffer less for it. :(

### Final Scores

The final obstacle I ran into was with Privilege Escalation. It wasn't that the escalations were too difficult, but rather I did not have all of the different techniques down to a science at that point. Even though I got access to all but one box each time, I was unable to gather enough points because I missed at least one crucial escalation vulnerability.

My fourth attempt left me with only one privilege escalation standing between me and victory, before I ran out of time. I felt as though I was only minutes away from the escalation as well, which is perhaps the most frustrating failure I had experienced to date. So close and yet so far.

In the end, on my second, third and fourth exam attempts, I can only estimate that I earned somewhere around 65 points. I don't know how the scoring scale works, since OffSec keeps that information under tight wraps, but I can't help but speculate.

Each time, I came **so** close but just couldn't get over the finish line.

I can't say definitively whether I was unprepared for each subsequent attempt. I came so close to passing each time that it almost felt like being cheated in a way, especially the fourth attempt. Part of me realizes that your results can change depending on the types of boxes you are given for each exam attempt.

Sometimes you get lucky and see the same ones again, sometimes you get a full scramble of new hosts you've never seen. Some are harder than others to pop. Sometimes the path to root is one of the first methods you check for, sometimes it's the last thing you'd think to try.

That small measure of luck might make a 5% difference, but I know for a fact that the rest of it is all in your persistence, determination, and preparation -- big emphasis on preparation.

In the next part, I'll cover preparation and avoiding common mistakes on the exam in hopes that it can help you prepare yourself better so you don't have to face the exam a fifth time :P
