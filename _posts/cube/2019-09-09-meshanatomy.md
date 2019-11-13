---
layout: rigACubePost
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

Every rig has to move those Verts / Edges / Faces (`components`) by some means (connections, 
constraints, deformers etc), so it is important you know about them. 

*Note: If you can do learn some modelling all the better.*

These `components` of a 3D model are considered the guts of what makes up the 
 the `shape` of a 3D model.

A `shape` requires a `transform` to describe it's overall position in 3DSpace
(eg: the `position` that defines where all the components are usually determined
by a `transform` node). 

- We can manipulate a 3D object(mesh, model) via this `transform`.
- We can manipulate a 3D object(mesh, model) by deforming the `shape`.

`Transform's` apply the `position(translation)`, `orientation(rotation)` and `scale` of the 
3D model. Thus when we move an object(mesh, model) via it's `transform` we can `Translate`,
`Rotate` and `Scale` it directly. 

Though to change the look of the shape of the object, we usually have to change
the `components` of the 3D object, by modelling it to look different, or in our
case we have to `deform` it via the rig.

Another thing to note here is another sub structure of a 3D model called 
`UV's`.  These are the texture co-ordinate system used to map images to 
the faces of the model but they can be leveraged for other things rig related too.

Here is the default UV for the cube.

<img src="http://www.anim83d.com/images/examples/cube_uv.png" width="575" height="573" alt="uv">

Interesting how the gif `unfolds` the cube to the same shape. This is because
each `face` of the 3D model has a `UV` associated to it. 

You can use UV co-ordinates in rigging to for sticking things to a mesh, etc...

[.. onwards... 3DSpaces](2019-09-10-3dspaces.md)
