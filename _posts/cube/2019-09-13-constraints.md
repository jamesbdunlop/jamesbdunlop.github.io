---
layout: rigACubePost
title: "rigConstraints"
isPost: true
description: "Basic look at connecting A to B via middle man constraints"
category: rigACube
---

A constraint is your "middle man" between the control [and the cube.]

Essentially you are specifying a relationship between a ctrl [and the cube].

They come in a few forms!

It might all seem a bit confusing, but the name of the constraint is usually a good
indicator as to what kind of "relationship" you're creating between the control
and the target.

Here are some of the base constraints;
 
eg in Maya:
===========
- pointConstraint   = just position
- orientConstraint  = just rotations
- parentConstraint  = position / rotation
- scaleConstraint   = scale 
- surfaceConstraint = constrain to a surface (NURBS)
- pointOnPolyConstraint = constrain to a polygon surface

And so on....

eg in Houdini:
==============
- ParentBlend = setting x# ctrls as parents
- Point, orientation, scale, parent, and aim constraints are all types of 
transformation constraints. They can be controlled by the Blend Object, 
Object CHOP, or by hscript and python expressions.

And so on....

<img src="http://www.anim83d.com/images/examples/cube_constraint01.gif" width="640" height="480" alt="constraint">

All of these can help setup a control system for underlying systems such as
deformers / FK / IK chains etc

[.. onwards...Forward Kinematics](2019-09-14-forwardkinematics.md)
