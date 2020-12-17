---
layout: codePost
title: "GimIK Live Guide - 01 Basic Setup"
isPost: true
description: "Messing with a live guide on the grimIK"
usage: ""
lastUpdated: "29-02-2020"
category: generalMaya
---

So I decided to have a bit of fun in my spare time messing with the grimIK node and a guide that can be place and deleted.

This node is available here: <a href="https://github.com/kattkieru/grim_IK">Grim IK Github</a>
I compiled this against windows 10 for maya 2020.
 
You can grab that mll here if you don't want to build it yourself: <a href="http://anim83d.com/maya/grimIK.mll">grimIK.mll</a>

The setup
=========

Since I want the FK controls to align automatically, I'm going to build the IK setup first. These are the base nodes I need
to start with.

<center><img src="http://anim83d.com/images/examples/grimBase_ikNodeGraph.png" alt="baseNodes" width="767" height="246"></center>


Hook these up as the node expects.

<center><img src="http://anim83d.com/images/examples/grimBase_ikNodeGraphConnected.png" alt="baseNodes" width="688" height="606"></center>


Make sure the hierarchy in the scene reflects what the node expects.

<center><img src="http://anim83d.com/images/examples/grimBase_ctrlPlacement.png" alt="baseNodes" width="209" height="146"></center>


And I want some guides for the IK controls. So duplicate these and call them _guide_ctrl.

<center><img src="http://anim83d.com/images/examples/grimBase_guideCtrls.png" alt="baseNodes" width="219" height="66"></center>

*Note: We only need to do the IK as the FK is going to work itself out ***from*** the IK.*

We also need to adjust the length of the limbs [manually for now] to break the knee. At this stage I set the upperLength and lowerLength to 2.1 
to begin with.

Now we hook up the guide to the offsetParentMatrix of the rig controls, and position the guides as expected.

*Note: when we delete the guide these connections will be severed automagically.*<br>
*Note2: If maya has an issue that you delete and it drops the current value, you may need to script the delete to remove connections first,
keeping the current value, and then remove the guide elements.*

<center><img src="http://anim83d.com/images/examples/grimBase_guideCtrlsHookedToORP.png" alt="baseNodes" width="1289" height="663"></center>

<center><img src="http://anim83d.com/images/examples/grimBase_guidePlacement.png" alt="baseNodes" width="383" height="516"></center>

And in it's simplest form, we can now connect the outputs of the grimIK to the fk offsetParentMatrix to align these as expected, via 
the worldMatrix[0] of the hip, and the matrix of the knee and ankle.

<center><img src="http://anim83d.com/images/examples/grimBase_hookedFKORP.png" alt="baseNodes" width="847" height="639"></center>

Now we can create the cycle (in the guide only it will go when we delete the guide) by hooking the fk worldMatrix[0] to the grimIK node.

***Note: THESE connections won't delete unless you script them to. So you'll want to handle these being deleted along with the 
guide deletion accordingly!***

<center><img src="http://anim83d.com/images/examples/grimBase_hookedFKORP2.png" alt="baseNodes" width="802" height="673"></center>

*Note: Notice how the grimIK node aligns downX for the hip and knee? This is one of the handy things about setting up the guide
 like this. The FK ctrls will adopt this alignment automatically. Turn off Orient Tip Blend if you want the ankle to maintain  the same aligment as
 hip and knee* 

<center><img src="http://anim83d.com/images/examples/grimBase_hookedFKORP3.png" alt="baseNodes" width="382" height="967"></center>


<img src="http://www.anim83d.com/images/examples/grimBase_hookedFKORP3.png" alt="baseNodes" width="382" height="967" class="inline">

And that's about it.

You have a global root guide to move the rig. Position the foot and pole. So now we can start layering in some more complexity.

[.. onwards... Knee](2020-07-22-grimIKLiveGuide02.md)


