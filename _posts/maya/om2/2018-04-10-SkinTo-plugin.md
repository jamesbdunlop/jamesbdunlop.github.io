---
layout: codePost
title: "skinTo as om2 plugin"
isPost: true
description: "Converted the script into a MPxCommand"
usage: "Step1: Save gist to valid plug-ins folder as jbd.py
        <p>Step2: In the script editor load the cmd cmds.loadPlugin('pathTo/plugin.py')</p>
        <p>Step3: Select some geo and run cmds.resetSkinClusters()</p>"
lastUpdated: "04-10-2018"
category: om2
---
So the args. Omg.
<br>
For some reason any of the standard approaches documented out there cause a kFailure trying to set args.
As it stands I can't get kStringObjects or kSelectionList working / parsed for the uvmaps=("map1", "map2")
argument. So I ended up putting those through as 2 sep string arguments and processing those internally.
<br>
Anyway here it is as a stand alone plugin. Oh and it's not production tested (yet) :)

{% gist f7e247c93fbbb0292482a8c39015b452 %}
