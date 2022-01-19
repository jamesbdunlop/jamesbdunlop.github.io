---
layout: rigACubePost
title: "deformers - blendshapes"
isPost: true
description: "Basic overview of what blendshapes are"
category: rigACube
---

Blend what?

Think of it as a transition from A to B. We have a sad face, and a 
happy face, when the face goes from happy to sad we're *blending* one shape
to the other.

The most familiar application of `blendshapes` is usually setting up rigs for 
facial animation, *though blendshapes can be used to solve a lot of other problems 
too.*

Characters can have hundreds of these shapes all defining little areas of movement
in the face that combine to create complete expressions. It is then up  to the rigger
to create an intuitive ctrl system to drive the blending.

Here is a look at a blendshape created for the cube, with 2 shapes it can
blend to.

<img src="/assets/examples/cube_blendshape.gif" width="640" height="480" alt="cubeSkin">

*Side Note: Each cube used as a different shape share the same structure
as the shape we want to deform. If we don't do this the computer won't know 
how to blend things that don't exist on the base model.*

So as you can imagine, if you have hundreds of these shapes for your 3D 
asset it can take some time to setup a good control rig. Most riggers learn
some basic coding skills to help reduce the time it takes to set things like 
this up.

**`Corrective shapes`** is a term you'll hear being used a bit of too.
It is a slightly more advanced topic, but essentially these are blendshapes too. 

But they're really not human readable (that means if you looked at the shape
you'd think it was just a mess) and that is because they are special shapes 
that are used to fix tricky deformation issues in, for eaxmple leg/arm FK/IK setups, 
and the corrective shapes get driven by rotation values / psd drivers(see below) 
from the rig automatically, animators would never need to worry about the 
corrective shapes.

Correctives come with their own setup implications and things such 
as the [braveRabbit weightDriver](http://www.braverabbit.com/weightdriver/) tools
get written to help reduce the pain involved in managing these kinds of workflows.

If you're starting out, it will take some time before you really start needing
corrective shapes, but it's important to know of the concept.

[.. onwards... ]()
