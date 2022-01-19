---
layout: codePost
title: "GimIK Live Guide - 01 Basic Setup"
isPost: true
description: "Messing with a live guide on the grimIK"
usage: ""
lastUpdated: "27-02-2021"
category: generalMaya
---

So I decided to have a bit of fun in my spare time messing with the grimIK node and a guide that can be place and deleted.

I started this a full year ago and have been too busy so I figured I might truncate it a bit and throw some concepts out there anyway.

This node is available here: <a href="https://github.com/kattkieru/grim_IK">Grim IK Github</a>
I compiled this against windows 10 for maya 2020.
 
You can grab that mll here for maya 2020 if you don't want to build it yourself: <a href="http://anim83d.com/maya/grimIK.mll">grimIK.mll</a>


Bare bones setup
================

No pun intended. 

Later on we will want the FK controls to align automatically via the guide so we're going to build the IK setup first! 

*The FK is going to work itself out ***from*** the IK. It will create a cycle in the guide only.*

These are the base nodes we need to start...

<center><img src="/assets/examples/grimBase_ikNodeGraph.png" alt="baseNodes" width="767" height="246"></center>


...hook these up as expected...

<center><img src="/assets/examples/grimBase_ikNodeGraphConnected.png" alt="baseNodes" width="688" height="606"></center>


...and make sure the hierarchy in the scene reflects what the node expects...

<center><img src="/assets/examples/grimBase_ctrlPlacement.png" alt="baseNodes" width="209" height="146"></center>


Guiding the IK controls:
------------------------

Now we want some guides for the IK controls. 

Simply duplicate the IK ctrls for these and rename them with _guide_ctrl.

<center><img src="/assets/examples/grimBase_guideCtrls.png" alt="baseNodes" width="219" height="66"></center>

We also need to adjust the length of the limbs [manually for now] to break the knee. At this stage I set the upperLength 
and lowerLength attributes on the grimIK node to 2.1 to begin with.

Now we can connect the [guides].worldMatrix[0] to their respective IK_ctrl.offsetParentMatrix, and position the guides as expected.

*Note: when we delete the guide these connections will be severed automagically.*<br>
*Note: If maya has an issue that you delete and it drops the current value, you may need to script up the deletion to remove connections first,
keeping the current value, and then remove the guide elements.*

<center><img src="/assets/examples/grimBase_guideCtrlsHookedToORP.png" alt="baseNodes" width="1289" height="663"></center>

<center><img src="/assets/examples/grimBase_guidePlacement.png" alt="baseNodes" width="383" height="516"></center>

Setting up the guided FK:
-------------------------

We'll need 3 curves for the FK so go ahead and make those. Hip, knee, ankle. And parent them in a hierachy.

We can connect the outputs of the grimIK in IK mode, to the FK_ctrl.offsetParentMatrix's to align them.

*Note: We can create this cycle without too much bother because it will vanish when we delete the guide.*

For the hip(root) we will have to account for the root's inverse space to make sure we don't double up transforms and get this to be
in the right space.

eg:

<center><img src="/assets/examples/grimBase_hookedFKORP.png" alt="baseNodes" width="847" height="639"></center>

***Note: THESE direct connections to offsetParentMatrix won't delete unless you script them to. So you'll want to handle 
these being deleted along with the rest of the guide delete accordingly!***

We can now hook the FK Ctrls into the grim IK Node.

<center><img src="/assets/examples/grimBase_hookedFKORP2.png" alt="baseNodes" width="802" height="673"></center>

*Note: Notice how the grimIK node aligns downX for the hip and knee? This is one of the handy things about setting up the guide
 like this. The FK ctrls will adopt this alignment automatically. Turn off Orient Tip Blend if you want the ankle to maintain  the same alignment as
 hip and knee* 

<center><img src="/assets/examples/grimBase_hookedFKORP3.png" alt="baseNodes" width="382" height="967"></center>

And that's about it for a bare bones guided setup. You have a global root guide to move the rig, guides to position the foot and pole.


<hr>

Adding more complexity:
======================

So now we can start layering in some more complexity as we have a bunch of other things we can consider for the fit to make 
life easier for someone else coming in to fit the rig to an asset.

Since the grimIK caters for characters lets stick to using character terms here;

- The knee position.
- Reset pose (if you're working in a bindPose and want to zero out the controls to a TPose)
- Global scaling
- Mirroring LR 

For this post lets investigate 2 approaches for the knee position and how that relates to the 2 limb lengths via some simple vector math.

Limb Lengths and the knee:
-------------------------

The two distances we're interested in here are the hip --> knee, and knee --> ankle.  We can whip up a quick bi-frost network 
to help us subtract the two vectors and glean the length of the resulting vector.

<center><img src="/assets/examples/bf_lenVec.png" alt="bf_lenVec.png" width="1048" height="310"></center>

... and noodle that into the graph like so;

<center><img src="/assets/examples/bf_lenVecNoodled.png" alt="/bf_lenVecNoodled.png" width="973" height="609"></center>

<center><img src="/assets/examples/liveGuide.gif" alt="liveGuide.gif" width="800" height="600"></center>

This definitely results in a knee that will slide the position of the grimIK but it does limit you to making sure you're in 
the plane at all times, and your bind pose must have legs straight down the line or you're going to have some problems! So
it's somewhat undesirable.


The knee poleVector from 3 points.
----------------------------------

How ever we can leverage the length setup, and then treat the hip, knee and ankle simply as 3 points in space and create a plane based off those with a 
vector shooting off through the knee to place the pole. *This means you can deal with character legs being off a less than desired angles etc.*

<center><img src="/assets/examples/kneePoleVecFromPoints.gif" alt="kneePoleVecFromPoints" width="393" height="516"></center>

If we dont' like the pivot from the center which swings that pole around a bit, we can make it travel with the knee.ty ...

<center><img src="/assets/examples/kneePoleVecFromPointsTravelling.gif" alt="kneePoleVecFromPoints" width="393" height="516"></center>

... the nodeEditor graph ...

<center><img src="/assets/examples/autoCalcPV_nodeEditorNetwork.png" alt="autoCalcPV_nodeEditorNetwork.png" width="875" height="772"></center>

... without all the debug/demonstration geo ...

<center><img src="/assets/examples/kneePoleVecFromPointsTravelling2.gif" alt="kneePoleVecFromPoints" width="709" height="556"></center>

This works nicely, we can hide the knee_poleVec_guide now if we want or leave it as an extra offset ctrl.

<center><img src="/assets/examples/3PointIntoLiveGuide.gif" alt="3PointIntoLiveGuide.gif" width="811" height="549"></center>

So yeah leaving it here for now. But as previously mentioned we can expand out this to include;

- Reset pose (if you're working in a bindPose and want to zero out the controls to a TPose)
- Global scale
- Mirroring L R
