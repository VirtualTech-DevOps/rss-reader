+++
title = """I wrote a very minimal terminal emulator from scratch, and I can use some feedback!"""
date = 2024-10-29T20:14:10.000Z
expiryDate = 2024-10-29T20:14:10.000Z
tags = ["reddit","cli"]
+++
[![I wrote a very minimal terminal emulator from scratch, and I can use some feedback!](https://preview.redd.it/th79p9bl6rxd1.jpeg?width=320&crop=smart&auto=webp&s=45bf4decca7029a8a86474ca6dadde1432f09f2c "I wrote a very minimal terminal emulator from scratch, and I can use some feedback!")](https://www.reddit.com/r/commandline/comments/1gf49p4/i_wrote_a_very_minimal_terminal_emulator_from/)

_Why?_

I was reading Linus' book, "Just for Fun" and he was talking about when was working with his Sinclair QL, he hated the text editor that came with it, because it was so slow, so he wrote his own in assembly which was significantly faster. Later he also was working on a terminal emulator with assembly to learn the architecture of his computer and he ended up writing his own kernel that we all know and love.

So I also wanted to make something like that, i thought I'd do a terminal, because you get some part of building a text editor and some experience with the os and important system calls. I obviously wasn't gonna use assembly, because I'm not good with it and even if I was I couldn't imagine writing more efficient code that would make it faster than C with all the compiler optimizations these days.

Anyways it's currently at a very basic level, it currently doesn't even support scrolling and I'm implementing that, this very moment. I was thinking of having a circular connected lines of text, so that if the last line is filled, the next line is stored in the first (0th) line and then the original line at that place must be stored somewhere? Idk i want to do something quick, and don't wish to move memory and create more memory everytime i have to scroll

I've used Pty/pseudoterminals to emulate shell, thanks to a tutorial by eduterm

I'll be happy to work on some suggestions.

submitted by [/u/Emotional-Zebra5359](https://www.reddit.com/user/Emotional-Zebra5359)  
[\[link\]](https://i.redd.it/th79p9bl6rxd1.jpeg) [\[comments\]](https://www.reddit.com/r/commandline/comments/1gf49p4/i_wrote_a_very_minimal_terminal_emulator_from/)

[[source]](https://www.reddit.com/r/commandline/comments/1gf49p4/i_wrote_a_very_minimal_terminal_emulator_from/)
