Implementing Custom Spells
==========================

Each spell should have a cast method with the following method header:

.. code-block:: py
  def cast(self, damageMultiplier, caster, target):
 
This is the method that is called whenever a spell is cast, and it is what makes spells actually work.

Each of the arguments in the cast method has its own purpose:

- ``damageMultiplier`` is a list with length 2 that contains:

  - The result of the hitbar (REDZONE/YELLOWZONE/GREENZONE) at index 0
  - The spell boost (as a float) at index 1
 
 - ``caster`` is either a ``BattlePlayer`` or a ``BattleEnemy`` (or any subclass of ``BattleEnemy``) object and represents the person that cast the spell
