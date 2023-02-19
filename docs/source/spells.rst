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

.. code-block :: py
  def cast(self, damageMultiplier, caster, target):
 
This is the method that is called whenever a spell is cast, and it is what makes spells actually work.

Each of the arguments in the cast method has its own purpose:

- ``damageMultiplier`` is a list with length 2 that contains:

  - The result of the hitbar (REDZONE/YELLOWZONE/GREENZONE) at index 0
  - The spell boost (as a float) at index 1
 
- ``caster`` is either a ``BattlePlayer`` or a ``BattleEnemy`` (or any subclass of ``BattleEnemy``) object and represents the person that cast the spell
- ``target`` is either a ``BattlePlayer`` or a ``BattleEnemy`` (or any subclass of ``BattleEnemy``) object and represents the person who is being targeted

Coding your first spell
-----------------------

Let's start making our first spell!

Start by making a new ``TestSpell`` class, derived from the ``Spell`` class.

Customize the attributes to make the spell have the desired name, hitbar, etc.

Finally, copy-paste the ``.cast`` method header, and we're ready to get started!

Printing Messages
~~~~~~~~~~~~~~~~~

Let's start by making our spell print a message when cast.

.. code-block :: py
   def cast(self, damageMultiplier, caster, target):
      printC("Hello World!", "B")
      
Now, whenever our spell is cast, it will print out "Hello World!" in blue text.

Checking the Result of the Hitbar
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Our spell shouldn't always do the same thing - the result should change based on the result of the hitbar!

This code will display a different message based on the hitbar location:

.. code-block :: py
   def cast(self, damageMultiplier, caster, target):
      if damageMultiplier[0] == REDZONE:
         printC("You hit in the |R|red|DG|!", "DG")
      elif damageMultiplier[0] == YELLOWZONE:
         printC("You hit in the |Y|yellow|DG|!", "DG")
      elif damageMultiplier[0] == GREENZONE:
         printC("You hit in the |G|green|DG|! Good job!", "DG")

You should note two important things about this code.

- The result of the hitbar is stored in ``damageMultiplier[0]``
- The constants ``REDZONE`` , ``YELLOWZONE`` , and ``GREENZONE`` should be used to check the result of the hitbar
