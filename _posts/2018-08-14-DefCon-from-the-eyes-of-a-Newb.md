---
layout: post
title:  "DefCon from the Eyes of a Newb"
date:   2018-08-14
categories:
- misc
---

# Welcome to DefCon 26

I had the opportunity and pleasure of getting to take a trip to Las Vegas during one of the largest hacking and security conferences in the world for the first time this past week. DefCon has always been one of those "Yeah, it'd be nice, but it'll never happen" kinds of experiences, or so I used to think. Thanks to the noble sacrifice of one heroic co-worker, who gave his plane seat that others may go forth in his stead, I had that chance.

Although I consider myself to be reasonably experienced in penetration testing, I anticipated the entire trip to be a profoundly humbling experience. I've always believed that if you want to learn from others, you need to approach them with the understanding that you don't know squat. I can honestly say, that attitude paid off in massive dividends as I walked among some of the brightest minds in Computer Science and discussed my favorite subjects with them. Defcon 26 was my first DefCon ever, and I'm determined to return again with much more to offer than what I took this time around. For the sake of doing it the justice it deserves, I'm here to tell everyone about the magical, tourist-trap wonderland of Las Vegas through the eyes of a bright-eyed 1337 wannabe in the midst of the chaos of the largest hacker event he has ever seen.

# The Arrival

After a grueling 5 hours on planes, sad between two fat, sweaty dudes who smelled like John Cena's gym shoes, 3 excessively awkward Uber/Lyft/Taxi rides trying to meander my way through conversations with the drivers, and wading through a sea of half-naked bird-women, buff, shirtless cowboys & mildly murderous Las Vegas traffic, I arrived at Caesar's palace at roughly 1500 hours, Thursday, August 9, 2018. The entire event held from the second & third floors of the casino conference center was teaming with what I can best describe as my favorite kinds of people on the planet.

The sheer number of hackers in one place was overwhelming, and it almost makes you want to rip the battery from your phone at first sight. Registration, as promised, is a cash-only exchange. No names, no ID, no problems. After receiving my adorable DefCon PCB badge, guidebook, and a sheet of some badass stickers to add to my collection, myself and the small group I was with meandered into the casino to appreciate the confused looks of the ordinary casino-goers as thousands of people in all black drab and blinking badges passed over the casino floor.

# Hacker Conduct

I'll first let it be said that I personally has not one single negative experience in my interactions with the other attendees. The entire conference was a place of cooperation, competition, and knowledge swapping that you simply don't get from your local hang-outs. Just to put it into perspective, DefCon printed more than 26,000 badges in anticipation of attendance. That's a TON of ideas and Wi-Fi packets floating around in one place. That being said, there was a lot of interactions, all positive in their own ways, with the other attendees. And ... to the best of my knowledge, I didn't get pwned, so... awesome experience! Honestly, everyone is so busy learning from each other and so obnoxiously paranoid that it wouldn't be worthwhile in my opinion to target conference attendees, but that's just me.

Plenty of other events did transpire while there that many would easily consider not so good, though not to the fault of any of the DefCon facilitators. In an unfortunate case of @0xMatt who was ejected from the casino for making a hypothetical [statement](https://twitter.com/0xMatt/status/1027661604559544320) on Twitter about who would make for ideal targets for a [cyber] attacks in a network so saturated as the DefCon NOC. The response from the hotel, who refused to allow a clarification of the context, was to threaten him with arrest, prosecution, issued a tresspass warning, and effectively put a restraining order (which has now been repealed). It was made very clear, day one, that the casino was prepared to have us jailed and prosecuted for anything they could consider a threat or misconduct.

Personally, I saw the whole thing as an absurd show of force on the hotel staff's part, much less than any kind of reasonable response to an actual threat. It was the first debacle of several during DefCon which thoroughly pissed off a lot of people.

For that reason ... many googly eyes were left behind in retaliation.

![Googly Socates](https://pbs.twimg.com/media/DkN8jPnU4AAvyFt.jpg:small)

# Casino Staff Conduct

Oh you knew I'd be talking about this one. Yeah, the casino staff started popping locks and busting into rooms like wannabe feds. Well... not literally, I'm sure they had keys, but such a gross invasion of your personal space is no less disturbing as if they kicked in the door. It was supposedly communicated (extremely vaguely, I'm sure) to the DefCon staff that room searches would be standard procedure. What actually ended up happening was unannounced searches and seizures of property by hotel staff from attendees rooms. Personal effects had been rummaged through when guests were not around, and when they were, protests to any search were responded to with a threat of eviction from the casino and potentially accompanying charges.

<blockquote class="twitter-tweet" data-lang="en"><p lang="en" dir="ltr">🚨 WARNING HACKERS 🚨<br>Ceasars staff are performing &quot;random&quot; security checks of rooms. If you opt out of room cleaning and used defcon discount they will check your room and WILL confiscate soldering irons + more!<br><br>Not a drill! Spread the word!<a href="https://twitter.com/hashtag/defcon?src=hash&amp;ref_src=twsrc%5Etfw">#defcon</a> <a href="https://twitter.com/hashtag/badgelife?src=hash&amp;ref_src=twsrc%5Etfw">#badgelife</a> <a href="https://twitter.com/hashtag/dc26?src=hash&amp;ref_src=twsrc%5Etfw">#dc26</a> <a href="https://twitter.com/hashtag/DEFCON26?src=hash&amp;ref_src=twsrc%5Etfw">#DEFCON26</a></p>&mdash; Andrew Wolf (@really_awolf) <a href="https://twitter.com/really_awolf/status/1028062678881693697?ref_src=twsrc%5Etfw">August 10, 2018</a></blockquote>
<script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

The cited excuse of the hotel staff was attributed to new policies in the wake of the Las Vegas shooting in October 2017. In my research of the whole scenario, the shooter at the time had been visited by room service & cleaning staff several times, meanwhile stockpiling weapons in his room unimpeded. The obvious ineffective strategy of strip-searching your guests' rooms; however, did not stop the hotel staff from confiscating soldering irons, lockpick sets, various hardware while searching through guests personal items without consent or cause regardless of whether the person was in the room. I was lucky in that my group and I chose to stay off the strip for a cheaper alternative. That choice (AirBnB) is now my only alternative, as far as I'm concerned.

For those attendees in the future, if you don't want your stuff searched and seized by the hotel, you're better off not staying there. The blatant [middle-finger](https://pbs.twimg.com/media/DkRtYrgVAAEwP0C.jpg) raised by Caesar's Palace in response to any privacy concerns punctuates the fact that they were far less interested in making sure their guests felt "safe" than they were in making sure they could [avoid law-suits](http://www.latimes.com/nation/la-na-mgm-lawsuit-vegas-shooting-20180717-story.html).

# The Talks

I was admittedly pretty selective of the talks I went to while at DefCon, although the few I did attend had a lot to offer. Two I will immediately call out were "God Mode Unlocked: hardware backdoors in x86 CPUs" from Christopher Domas and "SMBetray: Backdooring and breaking signatures" from William Martin.

The sheer effort put into the research that led these two researchers to discover their findings was unbelievable. Starting with Godmode Unlocked, Domas uncovered a series of secret instructions within x86 architecture CPUs that allowed him to execute any code he desired with kernel-mode privileges by setting what he called the "godmode bit." Upon release, I strongly recommend this talk to any hardware hackers and/or exploit developers that work this close to the assembler. Because of Domas' findings x86 architecture is , without a doubt, going to be kaboshed, especially in the event his PoC lands publicly.

For SMBetray, a tool developed by William Martin, it breathes new life into MitM attacks on Windows networks beyond SMB Relaying credentials. Simply put, Martin was successful in reverse engineering the SMB protocol's authentication scheme when **SMB Signature Signing** is in place. His discovery was shockingly underwhelming in the sense that the only thing required to validate the signature is the client-side username and password.

Again ... The only thing needed to break SMB Signature Signing is the username and password of the authenticating client. Although we may not necessarily have this at the time of the initial attack, and doesn't help us if we are trying to gain admin access to a target server, obtaining these credentials can often be as simple as feeding the challenged hash into JTR. In any case, where the credentials are known, SMBetray allows new possibilities like downgrading the authentication schema to NTLM from Kerberos, as well as intercepting and downloading files mid-stream over an authenticated SMB Session. Lastly, my favorite feature, is the ability to replace existing files with a .lnk equivalents to execute a shell on target systems.

For a thorough explanation on the tool and the abuse of this flaw, check out Will Martin's blog at [QuickBreach Blog](https://quickbreach.io) and feel free to download his awesome tool at [GitHub](https://github.com/quickbreach/SMBetray).

But the talks are just the beginning of what DefCon had to offer, and the real educating events of the con took place in unexpected corners.

# Badge (Hardware) Hacking

So, I made it my personal mission to hack the DefCon 26 badge before the con was over ... aaaaaand failed miserably. But that's somewhat to be expected. First of all, the badge was extremely unique and took the usual badge-hacking events to a new level by implementing a game within the badge itself.

<blockquote class="twitter-tweet" data-lang="en"><p lang="en" dir="ltr">Finally arrived at my first ever <a href="https://twitter.com/hashtag/DefCon?src=hash&amp;ref_src=twsrc%5Etfw">#DefCon</a><br>So it begins... <a href="https://t.co/6tkYkJGrDL">pic.twitter.com/6tkYkJGrDL</a></p>&mdash; True Demon (@TRUExDEMON) <a href="https://twitter.com/TRUExDEMON/status/1027707745779167232?ref_src=twsrc%5Etfw">August 10, 2018</a></blockquote>
<script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

My unfortunate failure to turn all of my Defcon letters green (got all red) led me to seek help from the hardware hacking village. After visiting, I quickly felt overwelmed by the sheer number of people at tables with fully stocked soldering kits and heat guns ripping the transistors & capacitors from their badges, soldering leads into the JTAG interfaces, and so on, I realized just how out of my league I was.

That feeling is something you get used to at DefCon. Even seasoned experts can quickly feel out of place in areas where your individual knowledge is trumped by just the sight of dozens of people with an intimate understanding of something you can scarcely grasp. My experience in the hardware hacking section was brief but magical. Just passing over the tables, asking questions from people working on various PCBs (not just badges) led me to new research I'd previously never dared to venture.

I eventually found myself seeking out new information that led me to a [GitHub repo](https://github.com/firelizzard18/dc26-badge) put up by a handful of hackers who managed to reverse engineer the *Badginal Intercourse* protocol. This is what was used by the DefCon badges to interface with each other, and even provided a tool to replay the packets used to unlock various portions of the badge puzzle.

The further I dug into the research, the more I found myself learning about hardware hacking than I'd even considered myself capable of. Just the asking "How the FORK does this thing even work!?" forced me to push my own boundaries and introduced me to a new kind of hacking I'd never seen myself trying. And **THAT** even by itself made the entire plane ride worth while ...

# COMPETITION!

Oh the competition! The CTF challenges, the Hack Fortress arena, the VulnLab Creation Station, and even a karaoke stage presented me with limitless choices to embarass myself in front of hackers across the planet. In the end, I stuck to my familiar places and ended up at two separate lockpicking stations. In the packet hacking village, of all places, I encountered the *Locksmith's Gauntlet* which was a race-to-the-finish style of lockpicking challenge where you were pitted against a 5-stage series of three locks each, ranked easy, medium, and hard.

Just the setup itself was impressive enough to draw a crowd as people came up to race against the clock for the highest score, earning points for locks based on their difficulty and earning bonuses for completing earlier than the 5-minute time limit. My score was none too impressive, looking at the top 5 who were able to crack 4 of the 5 hard locks in under 1 minute, and one person who finished the gauntlet in under a minute. Even so, it was an awesome way to demonstrate your deftness, speed, and precision with a pick & rake.

Next was the Warl0ck Gamez CTF, with points earned for one of three challenges:
1. Finish first in sudden-death Fortnight matches -- Not my forte (forgive me xD)
2. Complete hacking challenges in a jeopardy style online CTF, hacking various countries on a map.
3. Pick locks for points & swag, which I proudly earned 100% of rewards

![Dope Poker Chip](https://pbs.twimg.com/media/DkXZ3y3U8Ac-bG4.jpg:small)

All in all, the competition was a welcome distraction from the constant bombardment of knowledge and information that anyone simply cannot take in all at once.

# The Wrap-up

DefCon 26 was an unforgetable, humbling, amazing experience that really solidified all the reasons why I fell in love with hacking in the first place. Obviously, I'm looking forward to going back, hopefully without fear of being strip-searched at the doors of the casino and definitely with a new perspective. Walking in for the first time, I had no idea where to even start. Looking back, even after experiencing so much of the con, I missed out on a lot, but no moment was wasted. It's hard to make sense of all deez feelz, but I intend on comming back next year with something to offer a renewed hunger.

If there is one thing DefCon will remind you of, it will be that, for all of our differences and our wide bredth & gaps of knowledge, in the end *"we are all alike."*

-- See you at DefCon 27
