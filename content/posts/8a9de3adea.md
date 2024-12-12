+++
title = """mgitlog - Run `git log` across multiple repositories."""
date = 2024-12-10T15:38:15.000Z
expiryDate = 2024-12-10T15:38:15.000Z
tags = ["reddit","cli"]
+++
I’ve put together a small Bash script that _wraps git log_ and calls it across multiple repositories. Works on Mac, Unix, and WSL environments. Personally, I use it at the end of each week to gather my commits for time logging at work.

    mgitlog --mroot ~/projects \ --author=jane.smith@example.com --since "1 week ago" 

Repo: [https://github.com/thomasklein/mgitlog](https://github.com/thomasklein/mgitlog)

Feedback and suggestions are appreciated.

submitted by [/u/Educational\_Leg\_6624](https://www.reddit.com/user/Educational_Leg_6624)  
[\[link\]](https://www.reddit.com/r/commandline/comments/1hb4im2/mgitlog_run_git_log_across_multiple_repositories/) [\[comments\]](https://www.reddit.com/r/commandline/comments/1hb4im2/mgitlog_run_git_log_across_multiple_repositories/)

[[source]](https://www.reddit.com/r/commandline/comments/1hb4im2/mgitlog_run_git_log_across_multiple_repositories/)
