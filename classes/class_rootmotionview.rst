:github_url: hide

.. DO NOT EDIT THIS FILE!!!
.. Generated automatically from Godot engine sources.
.. Generator: https://github.com/godotengine/godot/tree/master/doc/tools/make_rst.py.
.. XML source: https://github.com/godotengine/godot/tree/master/doc/classes/RootMotionView.xml.

.. _class_RootMotionView:

RootMotionView
==============

**Inherits:** :ref:`VisualInstance3D<class_VisualInstance3D>` **<** :ref:`Node3D<class_Node3D>` **<** :ref:`Node<class_Node>` **<** :ref:`Object<class_Object>`

Editor-only helper for setting up root motion in :ref:`AnimationTree<class_AnimationTree>`.

Description
-----------

*Root motion* refers to an animation technique where a mesh's skeleton is used to give impulse to a character. When working with 3D animations, a popular technique is for animators to use the root skeleton bone to give motion to the rest of the skeleton. This allows animating characters in a way where steps actually match the floor below. It also allows precise interaction with objects during cinematics. See also :ref:`AnimationTree<class_AnimationTree>`.

\ **Note:** ``RootMotionView`` is only visible in the editor. It will be hidden automatically in the running project.

Tutorials
---------

- `Using AnimationTree - Root motion <../tutorials/animation/animation_tree.html#root-motion>`__

Properties
----------

+---------------------------------+---------------------------------------------------------------------+---------------------------+
| :ref:`NodePath<class_NodePath>` | :ref:`animation_path<class_RootMotionView_property_animation_path>` | ``NodePath("")``          |
+---------------------------------+---------------------------------------------------------------------+---------------------------+
| :ref:`float<class_float>`       | :ref:`cell_size<class_RootMotionView_property_cell_size>`           | ``1.0``                   |
+---------------------------------+---------------------------------------------------------------------+---------------------------+
| :ref:`Color<class_Color>`       | :ref:`color<class_RootMotionView_property_color>`                   | ``Color(0.5, 0.5, 1, 1)`` |
+---------------------------------+---------------------------------------------------------------------+---------------------------+
| :ref:`float<class_float>`       | :ref:`radius<class_RootMotionView_property_radius>`                 | ``10.0``                  |
+---------------------------------+---------------------------------------------------------------------+---------------------------+
| :ref:`bool<class_bool>`         | :ref:`zero_y<class_RootMotionView_property_zero_y>`                 | ``true``                  |
+---------------------------------+---------------------------------------------------------------------+---------------------------+

Property Descriptions
---------------------

.. _class_RootMotionView_property_animation_path:

- :ref:`NodePath<class_NodePath>` **animation_path**

+-----------+---------------------------+
| *Default* | ``NodePath("")``          |
+-----------+---------------------------+
| *Setter*  | set_animation_path(value) |
+-----------+---------------------------+
| *Getter*  | get_animation_path()      |
+-----------+---------------------------+

Path to an :ref:`AnimationTree<class_AnimationTree>` node to use as a basis for root motion.

----

.. _class_RootMotionView_property_cell_size:

- :ref:`float<class_float>` **cell_size**

+-----------+----------------------+
| *Default* | ``1.0``              |
+-----------+----------------------+
| *Setter*  | set_cell_size(value) |
+-----------+----------------------+
| *Getter*  | get_cell_size()      |
+-----------+----------------------+

The grid's cell size in 3D units.

----

.. _class_RootMotionView_property_color:

- :ref:`Color<class_Color>` **color**

+-----------+---------------------------+
| *Default* | ``Color(0.5, 0.5, 1, 1)`` |
+-----------+---------------------------+
| *Setter*  | set_color(value)          |
+-----------+---------------------------+
| *Getter*  | get_color()               |
+-----------+---------------------------+

The grid's color.

----

.. _class_RootMotionView_property_radius:

- :ref:`float<class_float>` **radius**

+-----------+-------------------+
| *Default* | ``10.0``          |
+-----------+-------------------+
| *Setter*  | set_radius(value) |
+-----------+-------------------+
| *Getter*  | get_radius()      |
+-----------+-------------------+

The grid's radius in 3D units. The grid's opacity will fade gradually as the distance from the origin increases until this :ref:`radius<class_RootMotionView_property_radius>` is reached.

----

.. _class_RootMotionView_property_zero_y:

- :ref:`bool<class_bool>` **zero_y**

+-----------+-------------------+
| *Default* | ``true``          |
+-----------+-------------------+
| *Setter*  | set_zero_y(value) |
+-----------+-------------------+
| *Getter*  | get_zero_y()      |
+-----------+-------------------+

If ``true``, the grid's points will all be on the same Y coordinate (*local* Y = 0). If ``false``, the points' original Y coordinate is preserved.

.. |virtual| replace:: :abbr:`virtual (This method should typically be overridden by the user to have any effect.)`
.. |const| replace:: :abbr:`const (This method has no side effects. It doesn't modify any of the instance's member variables.)`
.. |vararg| replace:: :abbr:`vararg (This method accepts any number of arguments after the ones described here.)`
.. |constructor| replace:: :abbr:`constructor (This method is used to construct a type.)`
.. |static| replace:: :abbr:`static (This method doesn't need an instance to be called, so it can be called directly using the class name.)`
.. |operator| replace:: :abbr:`operator (This method describes a valid operator to use with this type as left-hand operand.)`
