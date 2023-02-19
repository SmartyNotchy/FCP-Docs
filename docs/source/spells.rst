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

Each spell should have a cast method with the following method header::

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

Let's start by making our spell print a message when cast::

   def cast(self, damageMultiplier, caster, target):
      printC("Hello World!", "B")
      
Now, whenever our spell is cast, it will print out "Hello World!" in blue text.

Checking the Result of the Hitbar
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Our spell shouldn't always do the same thing - the result should change based on the result of the hitbar!

This code will display a different message based on the hitbar location::

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

Printing Messages, Part 2
~~~~~~~~~~~~~~~~~~~~~~~~~

Now, let's start making our spells include the name of the caster & target!

This code builds on the last section's code, but this time it includes dynamic names::

   def cast(self, damageMultiplier, caster, target):
      if damageMultiplier[0] == REDZONE:
         printCastMessage("You fail to attack {}!", "{} fails to attack you!", caster, target)
      elif damageMultiplier[0] == YELLOWZONE:
         printCastMessage("You attack {}!", "{} attacks you!", caster, target)
      elif damageMultiplier[0] == GREENZONE:
         printCastMessage("You land a critical hit on {}!", "{} lands a critical hit on you!", caster, target)

There are also some important things to note about this code.

- ``printCastMessage`` is a helper function that makes writing cast messages a lot easier.

   - The first argument is a string, representing the printed message when the caster is the player. "{}" should be used ONCE in the string to represent the enemy's name. If it is necessary to include the enemy's name multiple times, use "{0}" instead.
   - The second argument is a string, representing the printed message when the caster is the enemy. "{}" should be used ONCE in the string to represent the enemy's name. If it is necessary to include the enemy's name multiple times, use "{0}" instead.
   - The third & fourth arguments should be the caster and the target.

- When messages are printed using ``printCastMessage`` , the base color will be dark gray "DG". To change this, write "|W|" or another string key at the beginning of each message.

.. note::
   
   Respect people's pronouns! If it is easier to use pronouns in a message
   
   i.e. "You make Pilliam confused, causing him to..."
   
   ALWAYS use they/them pronouns
   
   i.e. "You make Pilliam confused, causing them to..."
   
   This avoids having to manually checking each student's gender and changing the string accordingly, and ALSO avoids any potential issues with misgendering people.
