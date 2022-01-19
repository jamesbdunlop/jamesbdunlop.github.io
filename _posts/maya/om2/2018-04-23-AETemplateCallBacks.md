---
layout: codePost
title: "Testing AE Editor Template CallBacks"
isPost: true
description: "Adding a custom slider/callback to the attribute editor"
usage: "NA"
lastUpdated: "04-21-2018"
category: om2
---

<center><img src="/assets/examples/AECallBack.gif" w="640" h="480" alt="AECallBack"></center>

The above gif is the result of: <a href="https://knowledge.autodesk.com/support/maya/learn-explore/caas/CloudHelp/cloudhelp/2016/ENU/Maya/files/GUID-1BB52D1E-AA28-438E-A008-A0F4173D20FD-htm.html">Use callbacks in Attribute Editor templates</a>

Since one of my nodes can crash maya (if you change the value
below the currently set int as it tries to clean the outputs array and maya crashes if anything
is connected to the array[x]) I decided to see if I can leverage the callback
on the Attribute Editor view to clean the outputs dynamically to avoid the crash.

First up. I had to create the Template file. Which was easy enough with the bonus tools.

So using the attribute editor template builder I whipped up a quick template
and then edited that to reflect the < description language="cb" > requirements...

The final AEhermiteArrayCrvTemplate.xml file ended up with the following snippet in it ;
{% highlight c++ %}
<attribute name='outputCount' type='maya.long'>
        <label>Output Count</label>
        <description language="cb">py.AEOutCountChanged.AEOutCountChanged</description>
</attribute>
{% endhighlight %}

I saved that file into my base scripts/AETemplates folder [that maya can source from correctly].

I created a AEOutCountChanged.py file and made sure maya can see that too. Why? well the callback
line in the template xml stipulates I have a python file called AEOurCountChanged with a function called
AEOutCountChanged in it via the "py.AEOutCountChanged.AEOutCountChanged"


The interesting part (I found) is that this .py file actually OVERRIDES the
current AE view. It doesn't just inherit what is already there. So you have to make
sure you recreate the Attribute Editor's view as you expect it to be.
I ended up with the intSliderGrp UI element being the best choice.


Thus below is the gist of the final python file that changed the values dynamically for me without
crashing maya as the slider changes value in the attribute editor.

Another thing to note here is that this is attached to a specific AE VIEW. So if I do this on the default view,
maya crashes as expected.

Hope this sheds some light on the AE Callbacks etc. Definitely one to keep stashed somewhere for future.

<center><img src="/assets/examples/aeview.png" alt="AEView"></center>

{% gist 5861bd99d7f95772afcf8280d036888e %}

