---
layout: rigACubePost
title: "deformers"
isPost: true
description: "Overview of what it means to `deform' a 3d Mesh"
category: rigACube
---

Defomers, deform stuff! 

Essentially this means that the `components` (remember those vertexes, edges
faces?) of a 3d object get moved(deformed) in a certain way (twisted, bent, warped etc). 

There are a lot of different types of `deformers` and  they generally work 
on the `shape` of the `geometry`, so they "deform" the 3D Object's `components`
(`vertexes`, `edges`, `faces` etc) directly, rather than it's `transform`. 

Here's a shortlist (but not limited to);

- Twist
- Bend
- Wire
- Wrap
- Sine
- SkinCluster
    - Linear
    - Dual Quaternion
    - Implicit
- Flare
- Muscle
- Sculpt 
- Jiggle
- Lattice
- etc etc etc etc

Deformation is an interesting area. If you can master 3D deformation then
you're already in a great spot to deliver quality rigs. Sure you need to 
provide an **intuitive interface** TO the deformation for the Animatiors, but 
shit deformation is .... well very fn hard to hide from.

Each `deformer` has it's own math to deal with, it's own `inputs / outputs`, 
some might be VERY CPU intensive (thus most are being pushed to GPU as a result).

Deformation at the time of writing this imo **is king**. 

If you `deform` the `mesh` badly there is no amount of good timing / weighting 
/ posing an Animator can do to get a scene looking good! That arm just doesn't
look like an arm anymore cause the `deformation` is ... well... shit! :P

It's a pretty unforgiving area. 

- Great body rig, shit face rig btzz....

- Great face rig, terrible body rig, btzzz....

eg: The `deformation` below looks great huh... btzzz.....

<img src="/assets/examples/badDeform.png" width="506" height="459" alt="badDeform">

It's really important you get to understand as many `deformers` as possible and 
how to work with these to get all kinds of great results. 

"Do I put a slew of `lattice` deformers through my eyelids? or just use 
`joints`?` `blendshapes`?" etc 

There are many fun hack's / work around's / approaches out on the internet.

Some are FANTASTIC examples of how to bring your rig down to a **slow crawl**
while trying to solve what is otherwise **a straight forward math problem.**

Others are strokes of genius hacking together methods to produce a result 
that would other wise be impossible, and **still** run at a great speed on the CPU/GPU
helping deliver a feature rich rig. 

My Advice **be discerning!** when layering your `deformation stack`.
 
 **If you have a feature rich rig with hundreds of bells and whistles that 
moves so slowly its impossible to pose... yeah no.**

To that end we're going to cover some fundamental `deformation` approaches 
to get a basic rig up and running.

*Side note: Also just for fun, you can use `direct connections` or `constraints` to control
various aspects of `deformers`, such as `intensity`, `position`, `rotation` etc*

[.. onwards...deformation-skinning](2019-09-17-skinning.md)
