---
layout: codePost
title: "GimIK Live Guide - 02 The knee"
isPost: true
description: "Messing with a live guide on the grimIK"
usage: ""
lastUpdated: "29-02-2020"
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

So we can whip up a quick bifrost network to figure this out for us.

<center><img src="http://anim83d.com/images/examples/bf_lenVec.png" alt="baseNodes" width="1048" height="310"></center>

And noodle that into the graph like so;

<center><img src="http://anim83d.com/images/examples/bf_lenVecNoodled.png" alt="baseNodes" width="973" height="609"></center>


k going to leave this here for now, as I'm a bit swamped with work :(
