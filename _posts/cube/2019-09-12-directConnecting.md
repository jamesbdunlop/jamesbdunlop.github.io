---
layout: rigACubePost
title: "DirectConnecting"
isPost: true
description: "Basic look at connecting A to B directly"
category: rigACube
---

A `direct connection` is where you take the `attribute(s)` from A and
plug(connect) it DIRECTLY into an `attribute` of B. There is nothing 
else in between that connection.

A.v ---> B.v

Here is an example of taking the `translate`, `rotate`, `scale` attributes
from a control curve, and directly connecting them into the cube's `translate` 
attribute.

<img src="/assets/examples/cube_directConnect.gif" width="640" height="480" alt="cube_directConnect.gif right click open in tab">

Direct connecting has it's uses *(though it can become problematic with more
complex rigs and might need some special handling)*, but as far as technical
execution, it's about as basic as it gets!

Creating a connection between A and B is handled differently in applications
and you'll want to familiarize yourself with Nodes and the like, but usually
it's a simple click and drag operation, source to destination kind of thing.

[.. onwards... Constraints](2019-09-13-constraints.md)
