---
layout: codePost
title: "om2-Convert-ResetSkinClusters to a python MPxCommand Part 2"
isPost: true
description: "How I converted the script into a MPxCommand - Part2"
usage: "<br>Step1: Save gist to valid plug-ins folder as jbdResetSkinClusters.py  \
<br>Step2: In the script editor load the cmd cmds.loadPlugin('pathTo/plugin.py') \
<br>Step3: Select some geo and run cmds.resetSkinClusters()"
lastUpdated: "04-10-2018"
category: om2
---
And updated to be entirely om2 as gg AD the docs are ticky to nav and adding syntax() into the mix highlighted this as a major issue.
Hopefully this might provide some insights into converting some usfeul blog scripts out there to your own cmds.myNewScript() commands.

<h2>Step2: Making the resetSkin plugin:</h2>
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
<b>I'm not going to bother trying to break down every step I did as it'll be a
huge thread and lose the point, which is a more general scope of what is involved.</b>

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
#import os
import maya.cmds as cmds
#for path in os.getenv("MAYA_PLUGIN_PATH").split(";"):
#    print path
cmds.file(new=True, f=True)
names = ["plugName"]
for pluginName in names:
    cmds.unloadPlugin("{}.py".format(pluginName))
    cmds.loadPlugin("C:/fullpathto/{}.py".format(pluginName))

{% endhighlight %}

<br>
And here it is in all it's glory converted to a maya command that has a working undo!
So I don't have to copy paste a huge chunk of data into the script editor anymore.
I can load an unload it like a c++ plugin and call it using  cmds.resetSkinClusters()

{% gist b1b5ab6449aede037d0d532e7d106df1 %}
