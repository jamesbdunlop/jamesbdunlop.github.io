---
layout: codePost
title: "om2-ResetSkinClusters"
isPost: true
description: "Used to reset the bindPreMatrix for skinClusters after fixing joint positions on a bound mesh"
usage: "Please see the commented code at the bottom for usage"
lastUpdated: "02-02-2018"
category: om2
---

The 2 uses at the bottom.
- Perform the reset on ALL skinClusters
- Perform the reset on skinClusters found on selected geo.

{% gist 5d9f0eace48dc7654cbecb8b608bc57a %}
