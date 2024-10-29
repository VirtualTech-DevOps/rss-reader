+++
title = """SmartCat: my attempt at making the most efficient LLM tool for terminal dwellers"""
date = 2024-10-29T13:06:00.000Z
tags = ["reddit","cli"]
+++
Hey everyone, I wanted to share and get feedback on my pet project that quickly became a pillar of my current engineering workflow.

[https://github.com/efugier/smartcat/](https://github.com/efugier/smartcat/)

First of all, I am aware of other initiatives. Let me get straight to why this may be different, and why it works better for me than any other tool I've tried.

This tool makes LLMs available as text manipulation entities in the CLI; you pipe text in and you get some result. `smartcat` is designed to make this pattern and all its many applications as efficient and straightforward as possible.

You can pipe in a simple question, some text to reformat, explain a stack trace, refactor some code, write the v0 of some function to iterate on, a quick script etc.

In the end, with it being available in terminal and editor (vim, kakoune, helix... all [support piping selection into the CLI](https://github.com/efugier/smartcat/?tab=readme-ov-file#integrating-with-editors)), it completely eliminated the need for Copilot and other completion tools for me. I much prefer the workflow and control this offers.

Now, feature-wise, what are the highlights? - Minimalistic - Plug and play, behaves well in the terminal by default (no explanation or parasitic text) - Built with workflow efficiency as the top priority, minimizing keystrokes to get your job done - Being a good Unix terminal citizen, meaning it works well with streams and thus "integrates" natively with all terminal editors (vim, kakoune, helix) by piping the selection into it - Configurable prompts that can be tailored to specific and repetitive tasks (refactoring, testing, explaining an error in the stack trace) - Continue the last conversation to iterate or get a sligtly different result

More details (and workflow gifs) in the README.

As the target audience for this tool I would love for you to share feedback, especially on the documentation and README as it's always hard to accurately gauge how confusing things can be when you're the one that built it.

I hope you find it as useful as I do!

submitted by [/u/nanuqk](https://www.reddit.com/user/nanuqk)  
[\[link\]](https://www.reddit.com/r/commandline/comments/1geu57k/smartcat_my_attempt_at_making_the_most_efficient/) [\[comments\]](https://www.reddit.com/r/commandline/comments/1geu57k/smartcat_my_attempt_at_making_the_most_efficient/)

[[source]](https://www.reddit.com/r/commandline/comments/1geu57k/smartcat_my_attempt_at_making_the_most_efficient/)
