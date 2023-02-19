The Player
==========

BattlePlayer
------------

The ``BattlePlayer`` class represents the state of the player during a battle.
It has roughly the following definition:

.. code-block:: py

  class BattlePlayer:
    def __init__(self):
      self.name = "Unnamed Player" 
      self.isPlayer = True

      self.health = 100
      self.maxHealth = 100
      self.nextHitBoost = 1 # 1 = 1x mult, 1.5 = 1.5x mult, etc.
      self.nextHitBoostDuration = 0
      self.nextDamageReduction = 1 # 1 = 100% damage taken, 0.5 = 50% damage taken, etc.
      self.nextDamageReductionDuration = 0
      self.effects = []

      self.wands = [] # all unlocked wands
      self.selectedWand = None # currently selected wand (should be singular)
      self.spells = [] # all unlocked spells
      self.arsenal = [] # currently selected spells (up to 4)
      self.items = [] # all items in inventory

    def getName(self):
      # Gets the name of the player, colored

    def getNameWithEffects(self):
      # Gets the name of the player with status effect icons, colored

    def update(self):
      self.health = clamp(self.health, 0, self.maxHealth)
      return self.health != 0

    def attack(self, target):
      # Opens a dropdown menu asking the player for an action during a battle
      
    def takeDamage(self, amount):
      newAmount = int(self.nextDamageReduction * amount)
      self.health -= newAmount
      self.nextDamageReductionDuration -= 1
      if self.nextDamageReductionDuration <= 0:
        self.nextDamageReductionDuration = 0
        self.nextDamageReduction = 1
      return newAmount


Attributes
----------

``BattlePlayer.name``
~~~~~~~~~~~~~~~~~~~~~
Type: ``string``

The name of the player, shown when printing out the battle header

``BattlePlayer.isPlayer``
~~~~~~~~~~~~~~~~~~~~~~~~~
Type: ``boolean``

Whether the player is in fact a player.

This boolean will always be ``True`` for the ``BattlePlayer`` and should not be modified.

``BattlePlayer.health``
~~~~~~~~~~~~~~~~~~~~~~~
Type: ``int``

The current health of the player.

``BattlePlayer.maxHealth``
~~~~~~~~~~~~~~~~~~~~~~~~~~
Type: ``int``

The maximum health that the player can have (i.e. when using healing spells)

``BattlePlayer.nextHitBoost``
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Type: ``float``

How much the player's next spell cast will be scaled up/down.

At a value of 1, the player's next spell with have no scaling.

At a value of 1.5, the player's next spell will be 50% more effective (deal 50% more damage, heal 50% more health, etc.)

At a value of 2, the player's next spell will be 100% more effective

At a value of 0.5, the player's next spell will be 50% LESS effective (deal 50% less damage, etc.)

and so on.

``BattlePlayer.nextHitBoostDuration``
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Type: ``int``

How many turns the player's spell boost will last.

Once this reaches zero, ``BattlePlayer.nextHitBoost`` will be reset back to 1.

``BattlePlayer.nextDamageReduction``
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Type: ``float``

How much damage the player will absorb on the next hit.

At a value of 1, the player will take normal damage.

At a value of 0.5, the player will take 50% less damage.

At a value of 0.25, the player will take 75% less damage.

At a value of 2, the player will take 100% MORE damage.

and so on.

``BattlePlayer.nextDamageReductionDuration``
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Type: ``int``

How many hits the player's shield will last.

Once this reaches zero, ``BattlePlayer.nextDamageReduction`` will be reset back to 1.

``BattlePlayer.effects``
~~~~~~~~~~~~~~~~~~~~~~~~
Type: ``list``

A list of ``Effect``s (or subclasses of) that the player currently has.

Once an effect's duration expires, it will be removed from this list.
