---
layout: rigACubePost
title: "inverse kinematics"
isPost: true
description: "Basic overview of what inverse kinematics is."
category: rigACube
---

*"Inverse Kinematics is the inverse function/algorithm of Forward Kinematics. 
The Forward Kinematics function/algorithm takes a target position as the 
input, and calculates the pose required for the end effector to reach the
target position — the pose is the output."* - wikipedia

Inverse kinematics (`IK`) is used for rigging things such as arms, legs, robot parts,
heck even car rigs can have IK setups in them.

If you take your hand and pick something up, did you rotate your humerus
first... then your radius/ulna next... then your wrist, to get to what you were
picking up? Probably not. You just stuck your hand out and grabbed it.

Sure all those `bones` rotated and moved... into the position required for you
to pick up the object, but you drove your hand out first to pickup the item 
you wanted. 

This is the traditional sense of `IK` in a nutshell. You take a point at the 
end of the chain and you can pull it around, and the IK math solves an `effector`
to *reach for* that point in space, all the while it rotates all the elements 
in the chain to obtain the best possible positions to get that `effector` 
to the point in space that you're moving around.

Here's a look at that in Maya.

<img src="http://www.anim83d.com/images/examples/ikDemo.gif" width="538" height="465" alt="ikDemoGif">

There are a few ways to approach this. But for now what you see above is
a `joint chain` that has an `IK solver` attached to it*(because Maya's native
solver attaches to `joints`)*, but if you wrote your own `IKChain solver` you could
have the controls as `inputs` to your `node` and output the `joints` from the `node`
instead.

Regardless of HOW you're being forced to setup `IK` the concept of `IK` is 
essentially the same, and once you get a "handle" on it you'll be setting
up `IK` a lot (or using a tool to do it for you).

Maya has a few flavours of `IK solvers` and you're going to need to do
some study to learn what each do etc;

- `SingleChain` solver
- `RotatePlane` solver 
- `Spring` solver

*There is also a splineIK approach that uses a `curve(`spline`) to drive the `joint chain`.*

Here is our Cube (with more `subdivision`s to show the `deformation` clearer)
being driven by a `rotatePlane IK Solver` in Maya (which makes it act more 
like a shoulder `joint` at the base, with an elbow break 1/2 way down).

<img src="http://www.anim83d.com/images/examples/ikCube.gif" width="538" height="465" alt="cubeIKGif">

[.. onwards... deformers](2019-09-16-deformers.md)
