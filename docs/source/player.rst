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

The name of the player, shown when printing out the battle header
