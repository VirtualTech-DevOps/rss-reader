+++
title = """JQ help needed - output from multiple inner arrays to include info from outer array"""
date = 2024-11-15T16:33:36.000Z
expiryDate = 2024-11-15T16:33:36.000Z
tags = ["reddit","cli"]
+++
I'm trying to output multiple inner arrays, but list information from the outer array to make a table.

    { "Id": 1, "Name": "Name1", "Snapshots": [ { "Time": 1731677322, "SnapshotRaw": { "Boxes": [ { "SubId": "123", "Names": [ "SubNames1" ] }, { "SubId": "789", "Names": [ "SubNames2" ] } ] } } ] }, { "Id": 3, "Name": "Name2", "Snapshots": [ { "Time": 1731677331, "SnapshotRaw": { "Boxes": [ { "SubId": "456", "Names": [ "SubNames3" ] }, { "SubId": "012", "Names": [ "SubNames4" ] } ] } } ] } ] 

My JQ at the moment:

    jq -r '.[]|"\(.Id),\(.Name),\(.Snapshots[].SnapshotRaw.Boxes[].Names[]),\(.Snapshots[].SnapshotRaw.Boxes[].SubId)"' 

and the output is

    1,Name1,SubNames1,123 1,Name1,SubNames2,123 1,Name1,SubNames1,789 1,Name1,SubNames2,789 3,Name2,SubNames3,456 3,Name2,SubNames4,456 3,Name2,SubNames3,012 3,Name2,SubNames4,012 

I end up with inaccurate results. Ideally, it would be

    1,Name1,SubNames1,123 1,Name1,SubNames2,789 3,Name2,SubNames3,456 3,Name2,SubNames4,012 

Thanks for any help

submitted by [/u/CallMeGooglyBear](https://www.reddit.com/user/CallMeGooglyBear)  
[\[link\]](https://www.reddit.com/r/commandline/comments/1gs0d8n/jq_help_needed_output_from_multiple_inner_arrays/) [\[comments\]](https://www.reddit.com/r/commandline/comments/1gs0d8n/jq_help_needed_output_from_multiple_inner_arrays/)

[[source]](https://www.reddit.com/r/commandline/comments/1gs0d8n/jq_help_needed_output_from_multiple_inner_arrays/)
