---
layout: codePost
title: "om2-NodeEditorMenuManger"
isPost: true
description: "Used to manger node editor right click menus"
usage: "Please see the commented code at the bottom for usage"
lastUpdated: "02-15-2019"
category: om2
---
<a href ="https://github.com/jamesbdunlop/neMenuManager">GitHubRepo</a>
with all the current WIP files for this little side proj.

So the nodeEditor in Maya can register commands on nodes using
customInclusiveNodeItemMenuCallbacks: <a href="https://jamesbdunlop.github.io/om2/2018/04/21/nodeMenusNEd.html">Part01</a>

The problem I found with this is it's a bit hellish to manage (especially
when developing).
So this is a little post for about me mashing together an approach to
facilitate managing adding commands to the nodeEditor via a little
menuManager of sorts. Why? Well reloading these menus over and over
does not remove the previous load!

So you have to remove them before the reload. Having a little manager to keep
track of what is loaded etc can be pretty helpful with this.

## The idea ##

The idea here is to be able to add as many commands as I like to my
code base without having to maintain the manager `seeing' this code.
To do this I'm going to add a little factory module in to scan for a
specific class in a path, and from that create a cache of all the commands
I want to create menus for.
Then the manager itself leverages that cache to add the menus to the
NodeEditor in Maya.

## The Factory -Menu Cache ##

{% highlight python %}
import os, pprint
import importlib, inspect
import logging
logging.basicConfig()
logger = logging.getLogger(__name__)

BASE = os.path.dirname(__file__)
MENUSPATH = "{}/menus".format(BASE)


MENUCACHE = {}
def createMenuCache(path=MENUSPATH, pkg="menus"):
    """
    Recurisvely fetch all the .py modules in the menus folder and any
    classes defined as a menu and add these to the cache
    :param path: `str` path to the root menus folder
    :param pkg: `str` .separated path for the importlib.import_module to use
    """
    for module in os.listdir(path):
        if module == '__init__.py' or module.endswith(".pyc"):
            continue

        if module.endswith(".py"):
            mod = importlib.import_module(name=".{}".format(module[:-3]),
            package=pkg)
            for eachMenu in inspect.getmembers(mod, inspect.isclass):
                MENUCACHE[eachMenu[1].ID] = eachMenu[1]
        else:
            createMenuCache(path="{}/{}".format(path, module),
            pkg="{}.{}".format(pkg, module))

    logger.info("MenuCache:")
    pprint.pprint(MENUCACHE)

if __name__ == "__main__":
    createMenuCache(MENUSPATH)


{% endhighlight %}

What this is doing is setting the base path to */menus relative to the
current filepath the factory.py lives in*. Scan subfolders for the .py
files and looks to import the class(es) in each file.
Now this works because I have dictated a specfic structure for each command
to uphold.

Assumption(s):
- Classes in any of these menu modules ARE MENUS! I don't or won't have
any classes that are NOT inheriting nem_base.MenuBase!
- Functions in these modules are used by the Menu Classes.

## MenuBase() ##
Menu base is the base menu class used by the neMenuManager to create
the menus as expected in Maya.

{% highlight python %}
try: #try except for standalone testing only
    from maya import cmds
except ImportError:
    pass
from menus import typeIDs as nem_typeids
import logging
logging.basicConfig()
logger = logging.getLogger(__name__)


class MenuBase(object):
    ID = nem_typeids.BASEID
    MENUNAME = ""
    NODENAME = ""
    FUNCTION = ""

    def __init__(self, isRadial=False, radialPos=""):
        self.__isRadial = isRadial
        self.__radialPos = radialPos

    def menu(self):
        """

        :return: `function`
        """
        raise NotImplementedError("Not implemented")

    def isRadial(self):
        return self.__isRadial

    def radialPos(self):
        return self.__radialPos

    def menufunction(self):
        def menuCmd(ned, node):
            if cmds.nodeType(node) == self.NODENAME:
                try:
                    cmds.menuItem(label=self.MENUNAME,
                    radialPosition=self.radialPos(),
                    c=self.FUNCTION)
                    return True
                except RuntimeError:
                    logger.warning("Can not create a valid menu item
                    for {}".format(self.MENUNAME))
                    return False
            else:
                return False

        return menuCmd

{% endhighlight %}

## Creating a menu.py ##
So inheriting / overloading this class is then fairly straight foward
for me to use when adding a new menu item to the nodeEditor.

eg: menus/nodes/hermite.py
{% highlight python %}
def createOutputJnts(*args):
    # This is the function to execute in the class stored in the
    # FUNCTION const

class MENUNAME(nem_base.MenuBase):
    ID = nem_typeids.A_UNIQUE_INT
    MENUNAME = nem_typeids.A_UNIQUE_STR
    NODENAME = nem_typeids.A_UNIQUE_VALIDMAYANODENAME_asSTR
    FUNCTION = createOutputJnts

    def __init__(self):
        nem_base.MenuBase.__init__(self, isRadial=True, radialPos="S")
{% endhighlight %}

So Here each menu gets a unique typeID, Name, and a function to call.
No args handled atm.
And each class has a related mayaNode that they are linked to when you
invoke a menu call in maya by right clicking over a node in the nodeEditor

## The Manager ##

From there The manager only needs to leverage the MENUCACHE to generate
all the menus and provide a way to handle this information
eg: adding/removing menus from the nodeEditor as expected.

{% highlight python %}
from maya.app.general import nodeEditorMenus
import logging
import menuFactory as neFactory
logging.basicConfig()
logger = logging.getLogger(__name__)
"""
usage:
import sys
path = "T://software//neMenuManager"
if path not in sys.path:
    sys.path.append(path)

import neMenuManager as neMM
neM = neMM.NodeEditorMenuManager()
"""

class NodeEditorMenuManager(object):
    def __init__(self, autoLoadMenus=True):
        self.menus = nodeEditorMenus.customInclusiveNodeItemMenuCallbacks

        if autoLoadMenus:
            logger.info("AutoLoading nodeEditor menus now... {}".format(neFactory.MENUCACHE.keys()))
            for id, menu in neFactory.MENUCACHE.iteritems():
                self.addMenu(menu=menu)

    def addMenu(self, menu):
        """
        :param menu: `MenuBase` instance
        """
        self.menus.append(menu().menufunction())
        logger.info("{}|{} id:{}".format(menu.NODENAME, menu.MENUNAME, menu.ID))

    def iterMenuItems(self):
        """
        :return: `int` `list`
        """
        for id, data in neFactory.MENUCACHE.iteritems():
            yield id, data

    def removeMenu(self, menuid):
        """
        :param menuid: `int`
        :return: `bool`
        """
        logger.info("Removing id {}".format(menuid))
        for id, menu in neFactory.MENUCACHE:
            if id == menuid:
                self.menus.remove(menu.menufunction())
                neFactory.MENUCACHE.pop(menuid, None)
                logger.info("Successfully removed menu!")
                return True

        logger.warning("Failed to remove menu!")
        return False

    def __repr__(self):
        str = "menuItems:\n"
        for k, v in self.iterMenuItems():
            str += "\tid: {} | node: {} name: {} \t| isradial: {} | pos: {}\n".format(
                k, v[0].NODENAME, v[0].MENUNAME, v[0].isRadial(), v[0].radialPos())

{% endhighlight %}

When the user imports neManager the __init__.py calls for the cache to be
created

__init.py__
{% highlight python %}
## Cache all the menus on importing the NEdMenuManager
import NEdMenuManager.factory as nefactory
nefactory.createMenuCache()
{% endhighlight %}
