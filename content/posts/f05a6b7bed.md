+++
title = """Tool for batch downloading assets from GitHub, URLs, and more"""
date = 2024-10-17T21:35:09.000Z
tags = ["reddit","cli"]
+++
Hello everyone!

I've written an experimental (and in development) CLI tool for batch downloading assets mainly from GitHub, automatically matching the current platform. Suitable for statically compiled binaries, but can be used to extract assets too.

Project page: [https://github.com/idelchi/godyl](https://github.com/idelchi/godyl)

Code and documentation is still rather messy, but has been tested on Windows (AMD64), Linux (ARM64, ARM32) and Darwin (ARM64) for the set of tools demonstrated in the repository.

Inspired by _task_, _dra_, _ansible_, and having to set up multiple servers with a standard set of tools.

Can be used as

`godyl <owner>/<repo>`

or by using a _yaml_ file listing tools and conditions for matching and downloading.

Can be downloaded and tested in a docker container with:

    curl -sSL | sh -s -- -v v0.0 -o ~/.local/binhttps://raw.githubusercontent.com/idelchi/godyl/refs/heads/dev/scripts/install.sh 

followed by

`godyl —update`

or with

    go install github.com/idelchi/godyl/cmd/godyl@latest 

submitted by [/u/Hour-Pie7948](https://www.reddit.com/user/Hour-Pie7948)  
[\[link\]](https://www.reddit.com/r/commandline/comments/1g623ci/tool_for_batch_downloading_assets_from_github/) [\[comments\]](https://www.reddit.com/r/commandline/comments/1g623ci/tool_for_batch_downloading_assets_from_github/)

[[source]](https://www.reddit.com/r/commandline/comments/1g623ci/tool_for_batch_downloading_assets_from_github/)
