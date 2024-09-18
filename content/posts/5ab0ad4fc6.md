+++
title = """yTui, an ytfzf alternative ?"""
date = 2024-09-11T07:53:21.000Z
tags = ["reddit","cli"]
+++
[https://github.com/Banh-Canh/ytui](https://github.com/Banh-Canh/ytui)

Hello, just released my first cli in Golang ever. It aims to be an alternative to ytfzf.  
I'm planning to add some light feature to interact with youtube and your account.  
At the moment, It features:

*   **YouTube Search and Playback**: Search YouTube videos using keywords or retrieve videos from your history or subscribed channels, and play them directly in your local terminal video player.
*   **FZF Integration**: `ytui` features something similar the popular fuzzy finder `fzf` (powered by [https://github.com/ktr0731/go-fuzzyfinder](https://github.com/ktr0731/go-fuzzyfinder)) to allow fast and efficient video search results navigation. This makes it easy to browse through large lists of videos and pick the one you want to play.
*   **Video History Management**: Keep track of the videos you've watched using `ytui`. The tool logs your watch history in the `watched_history.json` file for quick reference later.
*   **Channel Subscription Support**: Search for videos from your subscribed YouTube channels or specify channels in the configuration file.
*   **OAuth Authentication**: Securely authenticate with YouTube using OAuth to access your personal subscriptions. The tool leverages the YouTube Data API to fetch data related to your account.
*   **Terminal-Based User-interface**: Allowing you to search for videos directly from the terminal environment.
*   **Customization**: Adjust settings through a configuration file located in `$HOME/.config/ytui/config.yaml` to tailor the tool to your preferences, such as channel subscriptions and OAuth credentials.

submitted by [/u/NghangPho](https://www.reddit.com/user/NghangPho)  
[\[link\]](https://www.reddit.com/r/commandline/comments/1fe4pxf/ytui_an_ytfzf_alternative/) [\[comments\]](https://www.reddit.com/r/commandline/comments/1fe4pxf/ytui_an_ytfzf_alternative/)

[link](https://www.reddit.com/r/commandline/comments/1fe4pxf/ytui_an_ytfzf_alternative/)
