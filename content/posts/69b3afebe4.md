+++
title = """I created a TaskWarrior type tool but for scheduling"""
date = 2024-10-21T16:02:25.000Z
tags = ["reddit","cli"]
+++
Several years ago, I dealt with the hassle of having to create a weekly schedule that takes into account all my classes, homework, and studying. If something new came up, it'd mess up the existing schedule.

What QuickSched does is it takes your tasks, events (i.e. time blocks, recurring/individual), and straightforward config options to custom build an entire schedule for you in an instant. Scheduling and timetabling are done dynamically as per your config options to allow optimal flexibility, fully synced with Google Calendar. So, it works as both a task manager and scheduler :)

Command entry was inspired by TaskWarrior, but ours goes a step further by supporting flexible argument without prefacing the type due to our custom built parser (it flows much more naturally).

**TaskWarrior:**

    task add Class2 +C2 recur:weekly 11:00-12:45 rec:mon,wed,fri 

**QuickSched:**

    event true @ mon wed fri 11-12:45 +C2 "Class2" 

Furthermore, QuickSched supports "dirty" timestamp expressions that allow for quick and easy entry:

    Timestamp Formats: - 9-2 (9:00am-2:00pm) - 9-2:15 (9:00am-2:15pm) - 9:-2:15 (9:00am-2:15pm) - 09:00am-02:15pm (9:00am-2:15pm) - 09:am-2:15pm (9:00am-2:15pm) - 9:am-2:15pm (9:00am-2:15pm) - 9-2:15pm (9:00am-2:15pm) 

Let me know what you think. I'm considering porting the project over to Go w/ Fyne for a proper GUI.

GitHub: [https://github.com/AndrewRoe34/quick-sched](https://github.com/AndrewRoe34/quick-sched)

submitted by [/u/Lanky\_Reward\_6037](https://www.reddit.com/user/Lanky_Reward_6037)  
[\[link\]](https://www.reddit.com/r/commandline/comments/1g8t9zl/i_created_a_taskwarrior_type_tool_but_for/) [\[comments\]](https://www.reddit.com/r/commandline/comments/1g8t9zl/i_created_a_taskwarrior_type_tool_but_for/)

[[source]](https://www.reddit.com/r/commandline/comments/1g8t9zl/i_created_a_taskwarrior_type_tool_but_for/)
