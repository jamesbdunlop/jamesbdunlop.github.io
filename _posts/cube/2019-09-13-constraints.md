---
layout: rigACubePost
title: "rigConstraints"
isPost: true
description: "Basic look at connecting A to B via middle man constraints"
category: rigACube
---

A `constraint` is your "middle man" between A and B. Lets assume for now that A is the 
`control curve` that Animators will use, and B is our Cube for now. *Though you 
can create constraints on a lot of different things...*

Essentially you are specifying a relationship between the `Ctrl` and the cube.
*"Hey Cube... you're doing this because the control is doing this."*

`Constraints` come in a few forms!

It might all seem a bit confusing, but the name of the `constraint` is *usually* a good
indicator as to what kind of "relationship" you're creating between the `control`
and the Cube.

Here are some of the base constraints;
 
eg in Maya:
===========
- `pointConstraint`   = just position
- `orientConstraint`  = just rotations
- `parentConstraint`  = position / rotation
- `scaleConstraint`   = scale 
- `surfaceConstraint` = constrain to a surface (NURBS)
- `pointOnPolyConstraint` = constrain to a polygon surface

And so on....

eg in Houdini:
==============
- `ParentBlend` = setting x# ctrls as parents
- `Point`, `Orientation`, `Scale`, `Parent`, and `Aim` constraints are all types of 
transformation constraints. They can be controlled by the Blend Object, 
Object CHOP, or by hscript and python expressions.

And so on....

<img src="/assets/examples/cube_constraint01.gif" width="640" height="480" alt="constraint">

All of these can help setup a control system for the Rig, controlling systems such as
`Deformers` / `FK` / `IK` chains etc

When using constraints in rigging, we usually do these on the parent of
the control so that way nothing interfers with the animators keyframes.

[.. onwards...Forward Kinematics](2019-09-14-forwardkinematics.md)
