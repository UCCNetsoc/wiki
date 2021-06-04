---
title: Discord Bot
description: The UCC Netsoc Discord Bot which allows UCC students to automatically register as a member for the Discord Server, allows committee members to send announcements to multiple mediums and more!
published: true
date: 2021-04-15T15:47:59.524Z
tags: 
editor: undefined
---

# Discord Bot

## Public commands
- `!online`: see how many people are online in minecraft.netsoc.co
- `!ping`: pong!
- `!help`: displays this message
- `!register`: registers you as a member of the server

## Committee Features

The bot provides committee users to post both *events* and *announcements* to `#announcements`, the website and optionally, twitter.

### Posting an event
To post both events and announcements, go to `#bot_events` in the committee server and run the `!event` command as seen in the template below.
```
!event "title" "yyyy-mm-dd" "description" 
```
Note that an image **must** be attached when posting an event.

### Posting announcement
To post an announcement, go to `#bot_events` and post the `!announce` as seen below

```
!announce TEXT
```
Not an image is optional but not required in this case

You can also use `!sevent` and `!sannounce` to not announce to everyone as well :)