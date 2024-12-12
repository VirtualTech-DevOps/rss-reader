+++
title = """[DevLog #01] Gmail-TUI: Replicating The Gmail-Web Experience In Terminal"""
date = 2024-11-09T22:30:21.000Z
expiryDate = 2024-11-09T22:30:21.000Z
tags = ["reddit","cli"]
+++
[![[DevLog #01] Gmail-TUI: Replicating The Gmail-Web Experience In Terminal](https://external-preview.redd.it/kpfK-umJzs1v0uVwhhICSciKj1koEojHxw0Qa5VuA2c.gif?width=320&crop=smart&s=6a209b6a7d38395024b93c82a81d3eab97170bf2 "[DevLog #01] Gmail-TUI: Replicating The Gmail-Web Experience In Terminal")](https://www.reddit.com/r/commandline/comments/1gnlscc/devlog_01_gmailtui_replicating_the_gmailweb/)

[Gmail-TUI](https://github.com/dev-vaayen/Gmail-TUI/) is a simple TUI application that aims to replicate the Gmail Web-UI in a TUI-Environment. Is this even possible? I don't even know yet but let's find out! Special thanks to Rivo for their [TUI Library](https://github.com/rivo/tview/tree/master).

[Composing & sending a mail using Gmail-TUI](https://i.redd.it/h7hl604u4yzd1.gif)

As shown above (or [here](https://i.imgur.com/ZkHHSZp.gif) if the GIF didn't load), today I was able to implement the composing and sending of Emails using [this SMTP guide](https://www.geeksforgeeks.org/sending-email-using-smtp-in-golang/). The source-code is available in the [Project-repository](https://github.com/dev-vaayen/Gmail-TUI/blob/main/README.md) and modifying the code to enhance the project is most welcome!

Some Background
===============

As scary as this is for me, here I am trying to do something new with my life: Publicly writing about my project so that I actually end up completing it and also hopefully getting the much needed feedback along the way!

Just a few days after I had installed Ubuntu, I lost the access to the GUI due to a failed and interrupted update. This led to me being forced to use the TTY-environment (started using the \`ctrl+alt+fkeys\` combination) and ending up feeling helpless for a long time as I had never used even the most basic Linux commands.

Months later, this experience led me to look into TUI or Terminal based User Interfaces, which run on Terminals and are like lighter versions of GUIs. This is where the idea of creating my own TUI-Application for Gmail came into mind as I was unable find one that could fit my use-case.

Required Features
=================

To complete this lack of TUI-Application, I would like the Gmail-TUI to borderline replicate the web-version of Gmail, allowing users to perform most of the core tasks by providing following features/functionalities in it:

*   A login page for entering email-ID and password
*   Composing and sending mails - **Implemented!**
*   Listing received emails with email-IDs in the Inbox
*   Opening the content of the received mail after clicking it
*   Viewing sent email in Sent-Box
*   A small panel on the left side to choose from the Compose, Inbox, Drafts, Sent buttons.

I will be trying to work on the login-page for now, where the user will enter their credentials, click on login and be redirected to the next page where they would be able to compose mails. Like the web-version, showing the Inbox after signing-in should be done but since I am still studying IMPS that will help with receiving emails, I will be using the Compose-mail section as the placeholder for now.

submitted by [/u/dev-vaayen](https://www.reddit.com/user/dev-vaayen)  
[\[link\]](https://www.reddit.com/r/commandline/comments/1gnlscc/devlog_01_gmailtui_replicating_the_gmailweb/) [\[comments\]](https://www.reddit.com/r/commandline/comments/1gnlscc/devlog_01_gmailtui_replicating_the_gmailweb/)

[[source]](https://www.reddit.com/r/commandline/comments/1gnlscc/devlog_01_gmailtui_replicating_the_gmailweb/)
