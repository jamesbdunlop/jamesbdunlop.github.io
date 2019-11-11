---
layout: default
title: "mesh anatomy"
isPost: true
description: "Atomics of a 3D mesh"
category: rigACube
---

Simple division 1 cube ( a cube that has no subdivisions )

<img src="http://www.anim83d.com/images/examples/cube_anatomy.gif" width="640" height="480" alt="anatomy">

- 8 Vertexes
- 12 Edges
- 6 [Quad] Polygon Faces ( 12 Triangles)

Why is this important? What has this got to do with rigging?
------------------------------------------------------------

Every rig has to move those Verts / Edges / Faces by some means (connections, 
constraints, deformers etc), so it is important you know about them. 

 Note: If you can do learn some modelling all the better.

These elements(components) of a 3D model are usually part of the shape 
that makes up the model.

A shape requires a `transform` to describe it's overall position in 3d space
(eg: the position that will move all the components at once is usually defined
by a transform). 

- We can manipulate a 3D object(mesh, model) via this transform.
- We can manipulate a 3D object(mesh, model) by deforming the Shape.

Transforms apply positions(translate), orientations(rotate), scale of the 
3D model. Thus when we move an object(mesh, model) via it's transform we can translate,
rotate, scale it. To change the core shape of the object's components we 
have to deform it.

Another thing of note here is something called UV's These are the texture
co-ordinate system used to map images to the faces of the model.

Here is the default UV for the cube.

<img src="http://www.anim83d.com/images/examples/cube_uv.png" width="575" height="573" alt="uv">

Interesting how the gif `unfolds' the cube to the same shape. You can use 
UV co-ordinates in rigging to for sticking things to a mesh, etc...

[.. onwards... 3DSpaces](2019-09-11-3dspaces.md)
