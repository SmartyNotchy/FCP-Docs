Implementing Custom Spells
==========================

This is a tutorial and reference document for making your own custom spells.

Prerequisites
-------------

Understanding basic ``Spell`` attributes
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Before starting this tutorial, you should know how to implement the basic attributes of ``Spell`` and what they do.

.. note::

   If you don't know how to do this, you're probably not named Pilliam/Akash/Arjun/Omkar (actually you might be named Omkar now that I think about it), so go away!
 
Color Printing
~~~~~~~~~~~~~~

All color printing should be done with ``printC`` .

``printC`` takes the following arguments:

- ``txt`` : the text to print. Should be a string.
- ``base_col`` : the starting color of the text. Useful for printing one-line messages with no color changes. Default value of "DG".
- ``end`` : the ending character of the text. Defaults to "\n".
   
All colors are represent by a string key that is one or two characters long:

- "R":  red
- "BY": bright yellow (use sparingly)
- "Y":  gold (use in place of "BY" for most cases)
- "LG": lime green
- "G":  green
- "LB": light blue
- "B":  blue
- "DB": dark blue
- "PU": purple
- "PI": pink
- "W":  white
- "DG": dark gray
- "BR": brown

In order to change color mid-string, type ``"|string key|"`` in the middle of the ``txt`` string, replacing ``"string key"`` with the string key of the desired character.

Examples:

- "|R|This text is red! |B|This text is blue!
- "|R|I can cha|B|nge color mi|R|d-sentence!
- "|R|I can even do r|Y|a|LG|i|G|n|B|b|DB|o|PU|w|PI|s!"

You can also do extra formatting, such as:

- "This text is \*italic\*!"
- "This text is ~~strikethrough~~!" (To be implemented)

Happy color printing!

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
