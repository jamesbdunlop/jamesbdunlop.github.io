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
Uses the worldMatrix[0] as an input directly. The only downside was the array for the ends
would need to be fixed to input an IDMatrix by default if nothing is supplied. Problem for another day.

<center><img src="/assets/examples/bifrost_drawVec.gif" alt="biFrost_drawVector"></center>

