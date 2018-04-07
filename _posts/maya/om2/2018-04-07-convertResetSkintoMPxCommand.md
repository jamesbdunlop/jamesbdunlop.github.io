---
layout: codePost
title: "om2-Convert-ResetSkinClusters to a python MPxCommand"
isPost: true
description: "How I converted the script into a MPxCommand"
usage: "<br>Step1: Save gist to valid plug-ins folder as resetSkinClusters.py  <br>Step2: In the script editor load the cmd cmds.loadPlugin('{}.py'.format(pluginName)) <br>Step3: Select some geo and run cmds.resetSkinClusters()"
lastUpdated: "04-07-2018"
category: om2
---
Hopefully this might provide some insights into converting some usfeul blog scripts out there to your own cmds.myNewScript() commands.
<br>
<h3>Step 1: Gathering Info</h3>
Doing a quick google. Here's the info I found to use:
<li><a href="https://www.youtube.com/watch?v=BZyXe3MhEyI">Maya Python Plugin Overview P1</a>
<li><a href="https://www.youtube.com/watch?v=v1d8fCtIROI">Maya Python Plugin Overview P2</a>
<li><a href="http://docs.autodesk.com/MAYAUL/2014/ENU/Maya-API-Documentation/index.html?url=files/GUID-B968733D-B288-4DAF-9685-4676DC3E4E94-1.htm,topicNumber=d30e34174">Your First Maya Python Plug-in</a>
<li><a href="https://help.autodesk.com/view/MAYAUL/2018/ENU/?guid=__py_ref_class_open_maya_1_1_m_px_command_html">MPxCommand Class info</a>

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
<br>
<br>
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
Why? Well what I'm aiming for is to have a base file I can copy paste from that gives
me the entire layout for a python plugin to leverage for future plugins.
<br>
You'll notice the above gist is already a lot of code, but it's the shell of what we need
for Maya to work out what is going on. It's a chunk of data to remember, so I'm stashing
it in a gist.

<br><br>
<h2>Making the resetSkin plugin:</h2>
Looking at the previous post <a href="https://jamesbdunlop.github.io/om2/2018/01/31/resetSkinClusters.html">here</a>
I only have the one function resetSkinClusters(skinClusters=[])
<br>
So this should be fairly straight forward to convert. That resetSkinClusters() function will
essentially become the doIt().
<br>
<br>
Note: I needed to also make a decision, if I want to support a geoList=[]
argument or just handle a valid selection in the plugin.
For now I'm going to ignore the argument and assume users will want to
just select skinned geo and run the tool.

<br>
<br>
<b>I'm not going to bother trying to break down every step I did as it'll be a
huge thread and lose the point, which is a more general scope of what is involved.</b>

<br>
<br>

<h3>List of issue resolved along the way:</h3>
<li>I had to create the plug-ins folder in the base maya docs folder.
<br>
<i>"C:\Users\<username>\Documents\maya\<version>\plug-ins, or in one of the
directories defined by your MAYA_PLUG_IN_PATH environment variable."</i>
<br>

<li>The darn fileName became the plugin name when loading and unloading.
So be mindful of that or you'll get
<br>
"# RuntimeError: Plug-in, "resetSkinClusters.py", was not found on MAYA_PLUG_IN_PATH. # "

<li>Added a resolve method to the class to help find the selections and return these as expected.

<li>I forgot I just did all skinClusters so I needed to pull in my methods
from my om2 library _iterForSkinCluster and _findSkinCluster

<li>Since I used pyCharm to dev the script I needed to load and unload the
plugin in maya in maya to reflect changes made. So I wrote a little script to help
with that in the script editor:

{% highlight python %}
import maya.cmds as cmds
pluginName = "resetSkinClusters"
testFilePath = "pathToTestFile.ma"
## Force a new scene to cleanly unload the plugin
cmds.file(new=True, f=True)

cmds.unloadPlugin("{}.py".format(pluginName))
cmds.loadPlugin("{}.py".format(pluginName))
cmds.file(testFilePath, open=True)

## Then call the plugin for testing.
cmds.resetSkinClusters()
{% endhighlight %}

<br>
And here it is in all it's glory converted to a maya command that has a working undo!
So I don't have to copy paste a huge chunk of data into the script editor anymore.
I can load an unload it like a c++ plugin using  cmds.resetSkinClusters()

{% gist b1b5ab6449aede037d0d532e7d106df1 %}
