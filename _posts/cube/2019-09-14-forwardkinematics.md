---
layout: rigACubePost
title: "forward kinematics"
isPost: true
description: "Basic overview of what forward kinematics is."
category: rigACube
---

*"Kinematics is a branch of classical mechanics that describes the motion 
of points, bodies (objects), and systems of bodies (groups of objects) 
without considering the forces that cause them to move"* - wikipedia

Forward kinematics is when you have a `chain` of `controls` *(ctrls in a `hierarchy`)* 
eg: A|B|C|D in that when A is moved B C D follow, when B is moved C and D 
follow and so down the `chain'. 

Here are two examples of setting up an `FK chain`. One using parenting to create
the hierarchy and the other using constraints.

I'm using cubes here to clearly show what is going on, but these can be 
any type of `control Curve` / `transform`  etc..

example01:

<img src="/assets/examples/ABCD_hrc.png" width="147" height="90" alt="abcdHrc">

<img src="/assets/examples/fkHrc.gif" width="538" height="465" alt="abcdHRCGif">

<hr>

example02:

<img src="/assets/examples/ABCD_cst.png" width="181" height="202" alt="abcdCST">

<img src="/assets/examples/fkCST.gif" width="538" height="465" alt="abcdCSTGif">

As you can see both of these examples will result in a `control rig` that 
will behave in a forward [kinematic] `(FK)` way.

*Side note: we can then attach `joints` / `cluster`s etc to the results of 
these controls to alter the base shape `deform` the Cube.

Here is a look at that in action, *the control curves driving `joints` that are `skinned` to the
Cube* to change the shape of the cube (more on that later).

<img src="/assets/examples/cubeFKDeform.gif" width="538" height="465" alt="fkCubeDeformGif">

Another kinematic we use a lot is `Inverse Kinematics (IK)`.

[.. onwards... inverseKinematics](2019-09-15-inversekinematics.md)
