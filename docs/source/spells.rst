Implementing Custom Spells
==========================

This is a tutorial and reference document for making your own custom spells.

Prerequisites
-------------

Understanding basic ``Spell`` attributes
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Before starting this tutorial, you should know how to implement the basic attributes of ``Spell`` and what they do.

.. note::

   If you don't know how to do this, you're probably not named Pilliam/Akash/Arjun/Omkar, so go away!
 
Color Printing
~~~~~~~~~~~~~~

All color printing should be done with ``printC`` .



The .cast method: Basics
------------------------

Each spell should have a cast method with the following method header:

.. code-block:: py
  def cast(self, damageMultiplier, caster, target):
 
This is the method that is called whenever a spell is cast, and it is what makes spells actually work.

Each of the arguments in the cast method has its own purpose:

- ``damageMultiplier`` is a list with length 2 that contains:

  - The result of the hitbar (REDZONE/YELLOWZONE/GREENZONE) at index 0
  - The spell boost (as a float) at index 1
 
- ``caster`` is either a ``BattlePlayer`` or a ``BattleEnemy`` (or any subclass of ``BattleEnemy``) object and represents the person that cast the spell
- ``target`` is either a ``BattlePlayer`` or a ``BattleEnemy`` (or any subclass of ``BattleEnemy``) object and represents the person who is being targeted

Starting your first spell
-------------------------
