---
title: LemonSky / Essential Media and Entertainment
layout: workDone
roles:
    - Series Technical Directory(Pipeline Development)
    - Modelling(Sydney SailBoat)
    - Rigging TD(Boat rigging scripts)
isPost: false
---
<p>During my time on Bubblebath Bay I was involved in all kinds of roles, from operating / designing processes / process implementation / training / documentation etc etc.Clicking on read more will provide a further breakdown of roles performed during my time at Purple Pictures / Essential  Media.</p>
<p><span style="font-size: x-large;"><b>Pipeline Design</b></span></p>
<ul>
<li>Scoping necessary setup for handling the series.</li>
<li>Setting up Shotgun / Shotgun Pipeline Toolkit</li>
<li>Working out a LOD system using Autodesk Scene Assembly (Buggy in 2013.5 and Autodesk support really needs some work... serious work)</li>
</ul>
<p><img style="margin-center: 10px; width: 450px; height: 357px;" src="/assets/bbbayImages/ShotRequirements_sm.png" alt="" border="0" /> <img style="margin-center: 10px; width: 450px; height: 357px;" src="/assets/bbbayImages/LIGHTRequirements_sm.png" alt="" border="0" /></p>
<hr />
<p style="text-align: left;"><span style="font-size: x-large;"><b>Pipeline Build and Implementation</b></span></p>
<ul>
<li style="text-align: left;">Setup of github repos and TD dept in at Lemonsky for code sync</li>
<li style="text-align: left;">Shotgun publishing scripts for departments MDL, RIG, SRF, ANIM, LGHT.</li>
<li style="text-align: left;">Setting in place all sanity checks and background libs for asset publish handling</li>
</ul>
<p align="center"><img style="margin-center: 10px; width: 600px; height: 337px;" src="/assets/bbbayImages/shotgunConfig.png" alt="" align="middle" border="0" /></p>
<p align="left"><br /> <b>Various Shotgun Applications Coded:</b></p>
<ul>
<li style="text-align: left;"><b>Shotgun app</b> - tk-mayaplayblast<br /> Asset_step and shot_step aware, this app was used for playblasting turntables of assets or animations with the option to upload to directly to shotgun from Maya. <img style="width: 900px; height: 408px; display: block; margin-left: auto; margin-right: auto;" src="/assets/bbbayImages/tk-submit-mayaplayblast.png" alt="" align="middle" border="0" /></li>
<li style="text-align: left;"><b>Shotgun app</b> - tk-createShotcam<br /> Used to quickly create an appropriately named shot camera in scene based off the shotgun info.</li>
<li style="text-align: left;"><b>Shotgun app</b> - tk-duplicateAssets<br /> Used to duplicate assets such as Props or Characters for caching in animation. These can then accept surface variables during the lighting stages.</li>
<li style="text-align: left;"><b>Shotgun app</b> - tk-fetchShotAssets<br /> Used to query the related assets field of shots on shotgun, and import or reference etc these assets quickly into an animation scene for layout etc.</li>
<li style="text-align: left;"><b>Shotgun app</b> - tk-fetchCaches<br /> Used for bringing in the exported caches from animation.</li>
<li style="text-align: left;"><b>Shotgun app</b> - tk-fetchLightingShaders<br /> Used to rebuild the shaders for assets at lighting. On selected or for everything.</li>
<li style="text-align: left;"><b>Shotgun app</b> - tk-fetchUVs<br /> Fetches published UVs for assets if required. Handles multiple UV sets for alembic caches.</li>
<li style="text-align: left;"><b>Shotgun app</b> - tk-maayOcean<br /> Base setup of the FX tools for handling and setting up the maya ocean for the boats.</li>
<li style="text-align: left;"><b>Shotgun app</b> - tk-rebuildLIBSHD<br /> Rebuild LIB assets shading from their respective publishes in shotgun.</li>
<li style="text-align: left;"><b>Shotgun app</b> - tk-setupLighting<br /> Used to set the base renderglobals and lighting attributes.</li>
<li style="text-align: left;"><b>Shotgun app</b> - tk-rebuildCoreArchives<br /> Used to handle the vegetation approaches using core archives from mentalcore.</li>
</ul>
<hr />
<p><span style="font-size: x-large;"><b>Documentation / Wiki / In House training etc</b></span><br /> Responsible for writing tutorials and documentation for the tools and various processes built.</p>
<p> </p>