---
layout: default
title: "om2-ResetSkinClusters"
isPost: true
---
<br>So I finally decided to get this reset working in om2 to see if I can get it going a little faster.
<br>Hit a wee snag with the indices.. but nothing a good RTFM didn't fix.
<br>Seems to be working. Have tested this. But not run it over a full production rig yet so there might be some cases
<br>where it might fail.
<br>
Have put 2 uses at the bottom.
- Perform the reset on ALL skinClusters
- Perform the reset on skinClusters found on selected geo.

{% highlight python %}
"""2018 James Dunlop"""
import logging
import maya.cmds as cmds
import maya.api.OpenMaya as om2

logging.basicConfig()
logger = logging.getLogger(__name__)


def resetSkinCluster(skinClusters=[]):
    """
    :param skinClusters: A python string list of the names of the skinClusters in the scene.
    :return:
    """
    if not skinClusters:
        logger.warning("No valid skinClusters!")
        return

    mySel = om2.MSelectionList()
    for eachSKCls in skinClusters:
        mySel.add(eachSKCls)

    ## Set a MDGModifier up for setting values with
    myDGMod = om2.MDGModifier()

    ## Iter through all the skinclusters in the MSelectionList
    for x in range(mySel.length()):
        ## Get the dependNode and then the attributes of interest on the skinCluster
        ## Convert those attributes to MPlugs
        skClsMObj = mySel.getDependNode(x)
        skClsMFnDep = om2.MFnDependencyNode(skClsMObj)
        mtxAttr = skClsMFnDep.attribute("matrix")
        bindPreAttr = skClsMFnDep.attribute("bindPreMatrix")
        bindPrePlug = om2.MPlug(skClsMObj, bindPreAttr)
        matrixPlug = om2.MPlug(skClsMObj, mtxAttr)
        ## Find the bindPose node and get it's name
        dagPoseAttr = skClsMFnDep.attribute("bindPose")
        dagPoseMObj = om2.MPlug(skClsMObj, dagPoseAttr).source().node()
        dagPoseName = om2.MFnDependencyNode(dagPoseMObj).absoluteName()

        ## Get a list of all the valid connected indices in the matrix array now.
        indices = matrixPlug.getExistingArrayAttributeIndices()
        infuences = []
        for idx in indices:
            ## Get the inverseMatrix plug from the source and put that into the bindPreMatrix
            connectedMObj = matrixPlug.elementByLogicalIndex(idx).source().node()
            worldInverseMatrix = om2.MFnDependencyNode(connectedMObj).attribute("worldInverseMatrix")
            inverseMtxPlug = om2.MPlug(connectedMObj, worldInverseMatrix)
            InvMtx_matrixAsMObj = inverseMtxPlug.elementByLogicalIndex(0).asMObject()

            ## Store this in the myDGMod for exec after we have run through all the indices.
            myDGMod.newPlugValue(bindPrePlug.elementByLogicalIndex(idx), InvMtx_matrixAsMObj)

            ## And store the influence on the way through for the bindPose reset
            inf = om2.MFnDependencyNode(matrixPlug.elementByLogicalIndex(idx).source().node()).absoluteName()
            infuences.append(inf)

        ## Set all of the bindPreMatrix now.
        myDGMod.doIt()

        #################################################
        ## Now make sure the bindPose is fixed up or resetBindPose will fail.
        ## Don't know the om2 equiv of this! grrr
        cmds.dagPose(infuences, reset=True, n=dagPoseName)

        logger.info("Reset: {}".format(skClsMFnDep.name()))


#####################################
####### Reset all SkinClusters
import time
start = time.time()
resetSkinCluster(cmds.ls(type='skinCluster'))
logger.info("Reset took {} secs".format(start - time.time()))


#####################################
####### Reset skinCluster from selected Geo
start = time.time()
geo = cmds.ls(sl=True)
for eachGeo in geo:
    logger.info("##############################")
    logger.info("skinCluster reset for: {}".format(eachGeo))
    skCls = []
    getHistory = cmds.listHistory(eachGeo)
    for eachNode in getHistory:
        if cmds.nodeType(eachNode) == 'skinCluster':
            skCls.append(eachNode)
    if len(skCls) == 1:
        resetSkinCluster(skCls)
    else:
        logger.warning("Bad number of skinClusters for {}".format(eachGeo))
logger.info("Reset took {} secs".format(start - time.time()))

{% endhighlight %}