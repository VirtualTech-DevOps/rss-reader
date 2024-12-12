+++
title = """Built a Claude AI helper function for fish shell - because iTerm AI only supports OpenAI"""
date = 2024-11-07T04:21:41.000Z
expiryDate = 2024-11-07T04:21:41.000Z
tags = ["reddit","cli"]
+++
Hey folks! 👋

iTerm recently launched their AI feature where you can ask questions in natural language and get commands. But it only supports OpenAI's API, and I'm a Claude user. So I built a fish function that does the same thing!

What it does
============

*   Takes natural language input and returns the correct command for your system
*   Detects OS type and version (macOS/Linux) for accurate commands
*   Places the command on your prompt for review (no auto-execution)
*   Works with the latest Claude 3 models

Example usage
=============

    > ask-claude "flush DNS cache on my Mac" sudo dscacheutil -flushcache; sudo killall -HUP mDNSResponder > ask-claude "find large files taking up space" find . -type f -size +100M -exec ls -lh {} \; 

How to use it
-------------

Get a Claude API key from Anthropic Set these in your config.fish:

    set -gx CLAUDE_MODEL "claude-3-sonnet-20240229" set -gx CLAUDE_API_KEY "sk-ant-..." 

Drop the function in your fish functions directory

Check it out on GitHub: [ask-claude](https://github.com/MugunthKumar/ask-claude) PRs welcome! Planning to add support for more shells and Windows in the future.

submitted by [/u/fromblueplanet](https://www.reddit.com/user/fromblueplanet)  
[\[link\]](https://www.reddit.com/r/commandline/comments/1gli7c3/built_a_claude_ai_helper_function_for_fish_shell/) [\[comments\]](https://www.reddit.com/r/commandline/comments/1gli7c3/built_a_claude_ai_helper_function_for_fish_shell/)

[[source]](https://www.reddit.com/r/commandline/comments/1gli7c3/built_a_claude_ai_helper_function_for_fish_shell/)
