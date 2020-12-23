---
layout: codePost
title: "GimIK Live Guide - 02 The knee"
isPost: true
description: "Messing with a live guide on the grimIK"
usage: ""
lastUpdated: "23-12-2020"
category: generalMaya
---

So sure, it's all well and good to hook just the FK to follow the guide and consider it a live guide. But we have a bunch
of other things we can consider for the fit to make life easier for someone else coming in to fit the rig to an asset.

Since the grimIK caters for characters lets stick to using character terms here;

- The knee position.
- A reset pose (if you're working in a bindPose and want to zero out the controls to a TPose)
- Scaling considerations.
- Mirroring considerations.


We're now going to focus on how to do the knee position by adding a knee guide and working out how that relates to the
2 limb lengths via some simple vector math.

The two distances we're interested in here are the hip to knee, and knee to ankle.

So we can whip up a quick bi-frost network to figure this out for us.

<center><img src="/assets/examples/bf_lenVec.png" alt="bf_lenVec.png" width="1048" height="310"></center>

... and noodle that into the graph like so;

<center><img src="/assets/examples/bf_lenVecNoodled.png" alt="/bf_lenVecNoodled.png" width="973" height="609"></center>

<center><img src="/assets/examples/liveGuide.gif" alt="liveGuide.gif" width="800" height="600"></center>

The knee poleVector from 3 points.
----------------------------------

Another approach here is to just treat the hip, knee, ankle as three points in space and work out the pole position from 
that data.

eg:

<center><img src="/assets/examples/kneePoleVecFromPoints.gif" alt="kneePoleVecFromPoints" width="393" height="516"></center>

and if you dont' like the pivot from the center which swings that pole around a bit, you can make it travel with the knee.ty

<center><img src="/assets/examples/kneePoleVecFromPointsTravelling.gif" alt="kneePoleVecFromPoints" width="393" height="516"></center>

And this is what it looks like in the nodeEditor:

<center><img src="/assets/examples/autoCalcPV_nodeEditorNetwork.png" alt="autoCalcPV_nodeEditorNetwork.png" width="875" height="772"></center>

And a look at the setup without all the debug/demostration geo:

<center><img src="/assets/examples/kneePoleVecFromPointsTravelling2.gif" alt="kneePoleVecFromPoints" width="393" height="516"></center>

This way we can keep anim requests for hip / ik foot ctrls to be world aligned too. And the alignments for the FK ctrls can come
from a guide grimNode set to IK outputting the final results for those (possible more to come on that time permitting)

[back...](2020-07-22-grimIKLiveGuide01.md)

