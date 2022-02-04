---
title: "Discord Payload Recon"
# get-date -format yyyy-MM-ddTHH:mm:ss
date: 2022-02-03T18:09:52
draft: false
author: "Golgothus"
description: "Discord Payload Recon"
summary: "Friend of mine was a victim to some Discord malware which hijacked their user token."
tags: ["Blue Team"]
featuredImage: "/images/japan.jpg"
categories: ["Blue Team"]
---

Starting off I received a message from a friend, and they mentioned, "I am upset because, I got got". Not having any idea of what exactly they were alluding to, I asked what that meant. Long story short:

- Fellow Discord admin reached out asking if they wanted to test a game
- Sent an initial link which failed
- Sent follow-up link which was the payload
  - hxxps://cdn[.]discordapp[.]com/attachments/938523037442121761/938909145636163594/nearthedead_v1[.]2[.]1[.]exe

Due to interests in the matter, and this being a rather frequent issue, I went ahead and tried to do what I could to reverse the received payload. Went ahead and spun up my [Windows Sandbox](https://docs.microsoft.com/en-us/windows/security/threat-protection/windows-sandbox/windows-sandbox-overview) to take a look. Unfortunately this image comes up with no available tools, so I went ahead and ran the following for some baseline recon:

- Opened the browser, visited the site with the payload
- Opened up file explorer and went to `\\live.sysinternals.com\tools\`

> This is one of a few methods you can use to access Windows Sysinternals tools without having to use your browser https://docs.microsoft.com/en-us/sysinternals/

- Copied over the `strings.exe` tool from the SysInternals tools directory to my userprofile\Downloads folder
- Executed the following command `strings.exe -n 10 \<payloadfilename\> > output.txt`

From here I went ahead and reviewed the .txt file, at first I didn't find anything too much of interests, and I'll be honest I thought it was a C2 of some sort. About an hour or two had passed by this time and it was determined that the paylaod was a compiled NodeJS application.

I continued my search, looking for clear IOC's such as `server`, `post`, `ip`, however no luck. I had seen some references to different `github` repos, so I figured **why not**, and started my search for `git` and `github`, this is where things started to turn a little bit interesting. After completely reading through all the mentions of `github` I was able to see that the main repo this tool comes from is one by the name of  `pwnbetterdiscord`, which scrapes the user's Discord token information from the victim system.

Here's a small snippet of the output received from the previously executed `strings` file.

```
runningDiscords
startDiscord
pwnBetterDiscord
injectNotify
https://cdn.mertushka.me/public/stealer.js
C:\snapshot\app\structures\buildFiles\azmfo.js
\app-*\modules\discord_desktop_core-*\discord_desktop_core\index.js
writeFileSync
GrabushkaBTW
DiscordCanary.exe
discordcanary
DiscordDevelopment.exe
discorddevelopment
DiscordPTB.exe
listDiscords
taskkill /IM 
\Update.exe --processStart 
\BetterDiscord\data\betterdiscord.asar
readFileSync
api/webhooks
mertushkaisgod
:syringe: Inject Path
https://stealer.mertushka.me/images/logo.png
:detective: Successfull injection
stealer.mertushka.me
child_process
buffer-replace
hxxps://stealer[.]mertushka[.]me/api?id=170923455380848644
```

Some IOC's which may be of use:


```
SHA256          D91EE06532CB53AA4A4C2208D6143392F267CBDD7C843DD54055B46D6BB0F22B       Downloads\nearthedead_1.1.2.exe                      
https://www.virustotal.com/gui/file/d91ee06532cb53aa4a4c2208d6143392f267cbdd7c843dd54055b46d6bb0f22b

SHA256          999D5DAF7C66E1CFF81034E275D24635A599F001B8E7FD21BF8AA1683B618B2D       Downloads\nearthedead_v1.2.1.exe                            
https://www.virustotal.com/gui/file/999d5daf7c66e1cff81034e275d24635a599f001b8e7fd21bf8aa1683b618b2d                                          
```     

Unfortunately due to lack of my own experience and knowledge this is about as far as I've gotten. It sucks that it seems so easy to extract a user's token and not have much you can do from the victim side to stay safe, except just trying to be extra vigilant.

> Everyone says to be afraid of strangers, but sometimes it's the people we know who hurt us

Would have loved nothing more than to be able to find some more information on the attackers, but no dice. Hopefully my friend's account will be returned soon, and due to the frequency which items like this occur, I definitely feel for the responding engineers and analysts. I can also very easily say, I'd love nothing more than to watch them in their element, I imagine they see some really interesting things.

\- Signing off
