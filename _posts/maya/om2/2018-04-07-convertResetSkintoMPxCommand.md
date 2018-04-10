---
layout: codePost
title: "om2-Convert-ResetSkinClusters to a python MPxCommand Part 1"
isPost: true
description: "How I converted the script into a MPxCommand - Part1"
usage: ""
lastUpdated: "04-08-2018"
category: om2
---
I split this into 2 posts recently as getting the base boilerplate gist has grown trying to get args to work.
<br>
Also updated to be entirely om2 as the links below head more to om1 formatting.
<br>
Hopefully this might provide some insights into converting some useful blog scripts out there to your own cmds.myNewScript() commands.
<br>
<br>
<h3>Step 1: Gathering Info</h3>
Doing a quick google. Here's the info I found to use:
<li><a href="https://www.youtube.com/watch?v=BZyXe3MhEyI">Maya Python Plugin Overview P1</a>
<li><a href="https://www.youtube.com/watch?v=v1d8fCtIROI">Maya Python Plugin Overview P2</a>
<li><a href="http://docs.autodesk.com/MAYAUL/2014/ENU/Maya-API-Documentation/index.html?url=files/GUID-B968733D-B288-4DAF-9685-4676DC3E4E94-1.htm,topicNumber=d30e34174">Your First Maya Python Plug-in</a>
<li><a href="http://help.autodesk.com/view/MAYAUL/2018/ENU/?guid=__py_ref_class_open_maya_1_1_m_syntax_html">MSyntax</a>
<li><a href="http://help.autodesk.com/view/MAYAUL/2018/ENU/?guid=__py_ref_class_open_maya_1_1_m_arg_parser_html">MArgParser</a>
<li><a href="http://help.autodesk.com/view/MAYAUL/2018/ENU/?guid=__py_ref_class_open_maya_1_1_m_px_command_html">MPxCommand</a>

<br>
I'm choosing the more direct info as I want to <b>understand</b> this setup,
rather than just being spoon fed a walk through which I find my brain tends to
mindlessly "copy paste" from and subsequently struggles to remember wth I just did.
<br>
Yes that means you should check through all those links at some point to flesh out
your knowledge of whats going on below :)
<br>
Now a good place to start is rounding up the boilerplate code as AutoDesk
expects it.
<br>
Boilerplate code is the same code we can expect to use over and
over and over in every plugin we'll be writing ( with modifications to it for
each plugin!)

<br><br>
Maya Python Plugin Overview P1 has a good base to start from. How ever having
listened to the video while fleshing this post I'm also going to need;
<li>isUndoable()    -- Returns a boolean
<li>undoIt()        -- Maya calls this when undo is called eg: dgModifier.undoIt() <-- more complex commands may need more
<li>redoIt()        -- Maya calls this when redo is called eg: dgModifier.doIt() <-- more complex commands may need more

<br><br>
So I have added those into my boilerplate gist as well and I've added some additional
comments in the boiler plate found in the API Doc for furture refrence for what is what.
<br>
{% gist b661cbb0902b479e1d9bc859c2ee55ab %}

<br>
Why?
<br>
Well what I'm aiming for is to have a base file I can copy paste from that gives
me the entire layout for a python plugin to leverage for future plugins.
<br>
You'll notice the above gist is already a lot of code, but it's the shell of what we need
for Maya to work out what is going on. It's a chunk of data to remember, so I'm stashing
it in a gist. On to part2 of the post.
<a href=https://jamesbdunlop.github.io/om2/2018/04/07/convertResetSkintoMPxCommand02.html


