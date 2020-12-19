---
layout: codePost
title: "bifrost-BlendshapeQC"
isPost: true
description: "Network to help QC blendshapes stray weights."
usage: ""
lastUpdated: "19-09-2020"
category: bifrost
---
More Maya bifrost messing!

This time looking into using the network to output a shape of all connected blendshapes to show the moved verts clearly
so it's easy to spot stray undesired weights.

Super straight forward. Just color moved verts based on a tolerance.
We've found this kind of thing pretty helpful on previous gigs.

BiFrost network here: <a href="http://www.anim83d.com/maya/blendshapes_QC.rar">blendshapes_QC.rar</a>

<center><img src="http://anim83d.com/images/examples/bifrost_blendshapeMovedVerts.gif" alt="biFrost_rivet" width="511" height="251"></center>

Script up something to make all the connections for you and way we go!
