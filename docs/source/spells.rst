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
- ``end`` : the ending character of the text. Defaults to "\\n".
   
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
- "\-\-": the base color

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
   
   This avoids having to manually check each student's gender and changing the string accordingly, AND avoids any potential issues with misgendering people.

Writing Attack Spells
~~~~~~~~~~~~~~~~~~~~~

Time to make the spells deal damage!

This code snippet will cause the player to take damage::

   res = target.takeDamage(10)

The ``takeDamage`` method is implemented for ``BattlePlayer`` , ``BattleEnemy`` , and all subclasses of ``BattleEnemy`` .

It takes one argument ``amount`` , which is (shockingly) the amount of damage the target takes.

The method returns the amount of damage that the target ENDS UP taking, which is normally the same but may be lower/higher depending on the target's damage reduction shield.

Whenever you damage a target, you should always capture the return value in a variable for use in dialog.

This is a basic attack spell that always deals 10 base damage to the enemy::

   def cast( ... ):
      res = target.takeDamage(10)
      printCastMessage("{} takes %d damage!" % res, "You take %d damage!" % res, caster, target)
      
.. note::
   
   For brevity, from now on the arguments in the .cast method header will be shortened to "..."
   
.. note::
   
   Warning! It is highly recommended to use C-style string formatting to avoid conflicts with ``.format()`` in ``printCastMessage`` .
   
   You can read about C-style string formatting `here`_.
   
   Also note that it is not required to include "{}" in the string in printCastMessage; in other words, you can create cast messages that do not use any names!
   
.. _here: https://www.learnpython.org/en/String_Formatting

Now, let's make the damage dealt differ based on the hitbar result::

   def cast( ... ):
      if damageMultiplier[0] == REDZONE:
         printCastMessage("You mis-cast the spell!", "{} mis-cast the spell!", caster, target)
         notEffective()
      elif damageMultiplier[0] == YELLOWZONE:
         res = target.takeDamage(10)
         printCastMessage("{} takes %d damage!" % res, "You take %d damage!" % res, caster, target)
      elif damageMultiplier[0] == GREENZONE:
         res = target.takeDamage(20)
         printCastMessage("{} takes %d damage!" % res, "You take %d damage!" % res, caster, target)
         superEffective()

.. note::

   ``notEffective`` and ``superEffective`` are helper functions that print the message "It's not very effective..." in red text and "It's super effective!" in green text, respectively.
   
   For consistency, it is highly recommended to call ``notEffective`` upon a REDZONE hit and ``superEffective`` upon a GREENZONE hit.

Scaling with ``damageMultiplier[1]``
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Almost done! Now, we need to make the damage dealt scale with ``damageMultiplier[1]`` .

This part's simple: just multiply the arguent in ``target.takeDamage`` by ``damageMultiplier[1]`` !.

This code snippet deals a base of 10 damage, which scales up or down based on damageMultiplier[1]::
   
   target.takeDamage(10 * damageMultiplier[1])

With that in mind, this is the final refactored cast method::

   def cast( ... ):
      if damageMultiplier[0] == REDZONE:
         printCastMessage("You mis-cast the spell!", "{} mis-cast the spell!", caster, target)
         notEffective()
      elif damageMultiplier[0] == YELLOWZONE:
         res = target.takeDamage(10 * damageMultiplier[1])
         printCastMessage("{} takes %d damage!" % res, "You take %d damage!" % res, caster, target)
      elif damageMultiplier[0] == GREENZONE:
         res = target.takeDamage(20 * damageMultiplier[1])
         printCastMessage("{} takes %d damage!" % res, "You take %d damage!" % res, caster, target)
         superEffective()

Congratulations! You have now completed your first attack spell. This should be all you need to start making your very own attack spells!

Other Types Of Spells
---------------------

Attack spells are cool and all, but there are many other types of spells, too!

This section will cover various code snippets that can be used to make your spells do anything your heart desires!!

.. note::
   
   For legal reasons, that last sentence was a joke.
   
Healing Spells
~~~~~~~~~~~~~~

You can heal the caster with::

   res = caster.heal(10 * damageMultiplier[1])
  
The ``heal`` method will return the amount of health that has been healed, for use in dialog.

Defense/Weaken Spells
~~~~~~~~~~~~~~~~~~~~~

You can cast a shield for the caster with::

   caster.castShield(0.1, 1.2)

The ``castShield`` method takes two arguments:

- ``shield`` is a float that represents the shield to add. At 0.1, the caster's shield is boosted by 10%.
- ``cap`` is a float that represents the maximum shield that can be obtained with this spell. At 1.2, the caster cannot increase their shield past 20% by using this spell.  If the ``cap`` has already been reached, nothing happens (the shield is not reset to the cap.)

``castShield`` has no return value.

.. note::
   
   It is not recommended to scale the shield with ``damageMultiplier[1]`` , due to that requiring more advanced calculations and the existence of the shield cap.

.. note::

   ``castShield`` will always add (or subtract) from the target's current shield.
   
   This means that ``castShield`` can also be used to decrease the shield of the target (Weaken spells)::
     
      target.castShield(-0.1, 0.75)
   
   When ``cap`` is < 1, the shield will not go BELOW that number. (In the case of shields, ``cap`` can be negative; a ``cap`` of -1 represents a shield of -200%.)
   
   Note that negative shields are possible; an opponent with a shield of -10% (internally, 0.9) will take 10% more damage.
      
Boost/Weaken Spells
~~~~~~~~~~~~~~~~~~~

You can apply a boost for the caster with::

   target.castBoost(0.5, 2)

The ``castBoost`` method takes two arguments:

- ``boost`` is a float that represents the point scaling. At 0.5, the caster's spells are boosted by 50%
- ``cap`` is a float that represents the maximum boost that can be obtained with this spell. At 2, the caster cannot increase their boost past 100% by using this spell. If the ``cap`` has already been reached, nothing happens (the boost is not reset to the cap.)

``castBoost`` has no return value.

.. note::

   Since spell boosts directly affect ``damageMultiplier[1]`` , do NOT scale the boost with ``damageMultiplier[1]`` .
   
.. note::
   
   Similarly to ``castShield`` , ``castBoost`` adds or subtracts to any boost the target is under.
   
   So, ``castBoost`` can also be used to weaken a target's spells (i.e. call with arguments -0.1, 0.75). In the case of boosts, ``cap`` shoult NEVER be negative.

Combo Spells
~~~~~~~~~~~~

Don't be limited to one action per spell! Spells can carry out multiple actions at once.

For example, this code snippet attacks the target and heals the user for that amount::

   res = target.takeDamage(12 * damageMultiplier[1])
   res = caster.heal(res)
   
Bonus Spells
~~~~~~~~~~~~

Here are some extra code snippets that might come in handy.

"True Damage"
-------------

If you want the target to directly lose health and ingore their shield, manually reduce their health::

   target.health -= 10

Percentage Attacks
------------------

You can make a spell deal x% of the target's health. For example::

   target.takeDamage(0.05 * target.health) # Affected By Shield
   target.health -= 0.05 * target.health # "True Damage" Attack
   
