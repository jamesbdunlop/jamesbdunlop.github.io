---
layout: codePost
title: "Arrows Strands To Display Vectors"
isPost: true
description: "Some basic matrix concepts"
usage: ""
lastUpdated: "16-11-2021"
category: bifrost
---

So was a little bored tonight, decided to see what I could do to display a vector
using bifrost, which has a handy little create_arrow_strands I could leverage.

Results are not too bad, and could prob script up a little tool around it easily enough.
Uses the worldMatrix[0] as an input directly.


<center><img src="/assets/examples/bifrost_drawVec.gif" alt="biFrost_drawVector"></center>

And with a cleanup so default origins are IDMatrices, and a little connection script for selected, also showing
making the vec local to it's parent by plugging that into the originMtx.

<center><img src="/assets/examples/bifrost_drawVec2.gif" alt="biFrost_drawVector2"></center>
