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
With all the current WIP files for this little side proj.

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
import os
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
        if module == '__init__.py' or module.endswith(".pyc") or module == "base.py":
            continue

        if module.endswith(".py"):
            mod = importlib.import_module(name=".{}".format(module[:-3]), package=pkg)
            for eachMenu in inspect.getmembers(mod, inspect.isclass):
                menu = eachMenu[1]()
                MENUCACHE[menu.id()] = menu
        else:
            createMenuCache(path="{}/{}".format(path, module), pkg="{}.{}".format(pkg, module))
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
from maya import cmds
from menus import typeIDs as nem_typeids
import logging
logging.basicConfig()
logger = logging.getLogger(__name__)


class MenuBase(object):
    ID = nem_typeids.BASEID
    MENUNAME = ""
    NODENAME = ""
    FUNCTION = None

    def __init__(self, isRadial=False, radialPos=""):
        self.__isRadial = isRadial
        self.__radialPos = radialPos
        self._func = None

    def menu(self):
        """

        :return: `function`
        """
        raise NotImplementedError("Not implemented")

    def id(self):
        return self.ID

    def name(self):
        return self.MENUNAME

    def nodeType(self):
        return self.NODENAME

    def isRadial(self):
        return self.__isRadial

    def radialPos(self):
        return self.__radialPos

    def menufunction(self):
        if self.FUNCTION is None:
            return

        if self._func is None:
            def menuCmd(ned, node):
                if cmds.nodeType(node) == self.NODENAME:
                    try:
                        cmds.menuItem(label=self.MENUNAME, radialPosition=self.radialPos(), c=self.FUNCTION)
                        return True
                    except RuntimeError:
                        logger.warning("Can not create a valid menu item for {}".format(self.MENUNAME))
                        return False
                else:
                    return False
            self._func = menuCmd
            return menuCmd
        else:
            return self._func
{% endhighlight %}

## Creating a menu.py ##
So inheriting / overloading this class is then fairly straight foward
for me to use when adding a new menu item to the nodeEditor.

eg: menus/nodes/hermite.py
{% highlight python %}
def FUNCNAME(*args):
    # This is the function to execute in the class stored in the
    # FUNCTION const

class MENUNAME(nem_base.MenuBase):
    ID = nem_typeids.A_UNIQUE_INT
    MENUNAME = nem_typeids.A_UNIQUE_STR
    NODENAME = nem_typeids.A_UNIQUE_VALIDMAYANODENAME_asSTR
    FUNCTION = FUNCNAME

    def __init__(self):
        nem_base.MenuBase.__init__(self, isRadial=True, radialPos="S")
{% endhighlight %}
eg: <a href="https://github.com/jamesbdunlop/neMenuManager/blob/master/menus/nodes/hermite.py#L29">gitHub example</a>
So Here each menu gets a unique typeID, Name, and a function to call.
Note; No args are handled atm.
And each class has a related mayaNode that they are linked to when you
invoke a menu call in maya by right clicking over a node in the nodeEditor

## The Manager ##

From there The manager only needs to leverage the MENUCACHE to generate
all the menus and provide a way to handle this information
eg: adding/removing menus from the nodeEditor as expected.

{% highlight python %}
from maya.app.general import nodeEditorMenus
import logging
import menuFactory as ne_factory
logging.basicConfig()
logger = logging.getLogger(__name__)
"""
usage:
import sys
path = "T://software//neMenuManager"
if path not in sys.path:
    sys.path.append(path)

import neMenuManager as neMM
nedMenuManager = neMM.NodeEditorMenuManager()
for id, e in nedMenuManager.iterMenuItems():
    print(id)
"""


class NodeEditorMenuManager(object):
    def __init__(self, autoLoadMenus=True):
        self.menus = nodeEditorMenus.customInclusiveNodeItemMenuCallbacks
        self._ids = []
        if ne_factory.MENUCACHE:
            self._ids = ne_factory.keys()
            self.removeAll()
        else:
            logger.warning("Creating MENUCACHE now!")
            ne_factory.createMenuCache()

        if autoLoadMenus:
            logger.info("AutoLoading nodeEditor menus")
            for id, menu in ne_factory.MENUCACHE.iteritems():
                self.addMenu(menu=menu)
                self._ids.append(id)

    def addMenu(self, menu):
        """
        :param menu: `MenuBase` instance
        """
        if menu.menufunction() is not None:
            self.menus.append(menu.menufunction())
            logger.info("Added: {}|{} id:{} func: {}".format(menu.nodeType(), menu.name(), menu.id(), menu.menufunction()))

    def iterMenuItems(self):
        """
        :return: `int` `MenuBase() Instance`
        """
        for id, data in ne_factory.MENUCACHE.iteritems():
            yield id, data

    def removeMenu(self, menuid):
        """
        :param menuid: `int`
        :return: `bool`
        """
        logger.info("Removing id {}".format(menuid))
        if menuid in ne_factory.MENUCACHE.keys():
            try:
                self.menus.remove(ne_factory.MENUCACHE[menuid].menufunction())
            except ValueError:
                logger.warning("Failed to remove menuid: {} from maya's internal callback list!".format(menuid))
                return False

            ne_factory.MENUCACHE.pop(menuid, None)
            self._ids.remove(menuid)

            logger.info("Successfully removed menu!")
            return True

        logger.warning("Failed to remove menu!")
        return False

    def removeAll(self):
        """Remove all the menus before a reload!"""
        for eachID in self._ids:
            self.removeMenu(menuid=eachID)

    def currentCache(self):
        return ne_factory.MENUCACHE

    def __repr__(self):
        str = "menuItems:\n"
        for k, v in self.iterMenuItems():
            str += "\t id: {}".format(k)
            str += "\t name: {}".format(v.name())
            str += "\t nodeType: {}".format(v.nodeType())
            str += "\t isradial: {}".format(v.isRadial())
            str += "\t pos: {}\n".format(v.radialPos())
            str += "\t func: {}\n".format(v.menufunction())

        return str
{% endhighlight %}

