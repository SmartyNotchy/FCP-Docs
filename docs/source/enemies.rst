Enemies
=======

BattleEnemy
-----------

The ``BattleEnemy`` class represents the state of the enemy during a battle.

This class can and should be subclassed in order to allow for more functionality/different behaviors.

It has roughly the following definition:

.. code-block:: py

   class BattleEnemy:
    def __init__(self):
      self.name = "Generic Enemy"
      self.nameColor = "W"
      self.isPlayer = False

      self.health = 100
      self.maxHealth = 100
      self.nextHitBoost = 1
      self.nextHitBoostDuration = 0
      self.nextDamageReduction = 1
      self.nextDamageReductionDuration = 0
      self.effects = []
      
      # Other attributes that aren't important to implementing custom wands/spelsl

    def getName(self):
      return "|" + self.nameColor + "|" + self.name

    def getNameWithEffects(self):
      res = ""
      for effect in self.effects:
        res += effect.icon + "  "
      res += self.getName()
      return res

    def update(self):
      self.health = clamp(self.health, 0, self.maxHealth)
      return self.health != 0

    def onBattleStart(self):
      pass

    def onBattleWon(self):
      pass

    def onBattleLost(self):
      pass

    def attack(self, target):
      self.selectedWand.cast(random.choice(self.arsenal), self, target)
      for effect in target.effects:
        effect.effect(target)

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

Many of the attributes in ``BattleEnemy`` are functionally the same as for ``BattlePlayer``.

It is highly recommended to refer to the ``BattlePlayer`` documentation before reading this.

``BattleEnemy.name``
~~~~~~~~~~~~~~~~~~~~
Type: ``string``

The name of the enemy, shown when printing out the battle header

``BattleEnemy.isPlayer``
~~~~~~~~~~~~~~~~~~~~~~~~
Type: ``boolean``

Whether the enemy is in fact a player.

This boolean will always be ``False`` for the ``BattleEnemy`` and its subclasses and should not be modified.

``BattleEnemy.health``
~~~~~~~~~~~~~~~~~~~~~~
Type: ``int``

Functionally the same as for ``BattlePlayer`` .

``BattlePlayer.maxHealth``
~~~~~~~~~~~~~~~~~~~~~~~~~~
Type: ``int``

Functionally the same as for ``BattlePlayer`` .

``BattlePlayer.nextHitBoost``
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Type: ``float``

Functionally the same as for ``BattlePlayer`` .

``BattlePlayer.nextHitBoostDuration``
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Type: ``int``

Functionally the same as for ``BattlePlayer`` .

``BattlePlayer.nextDamageReduction``
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Type: ``float``

Functionally the same as for ``BattlePlayer`` .

``BattlePlayer.nextDamageReductionDuration``
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Type: ``int``

Functionally the same as for ``BattlePlayer`` .

``BattlePlayer.effects``
~~~~~~~~~~~~~~~~~~~~~~~~
Type: ``list``

Functionally the same as for ``BattlePlayer`` .

Methods
-------

``BattlePlayer.getName()``
~~~~~~~~~~~~~~~~~~~~~~~~~~
Return type: ``string``

Returns the name of the player, colored, to be used in dialog/battle headers.

``BattlePlayer.getNameWithEffects()``
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Return type: ``string``

Returns the name of the player, colored, along with effect icons. To be used in battle headers. Should NOT be used in normal dialog.

``BattlePlayer.update()``
~~~~~~~~~~~~~~~~~~~~~~~~~
Return type: ``boolean``

Clamps the player health to be in the range [0, BattlePlayer.maxHealth], inclusive.

Returns ``True`` if the player is alive (health > 0) and ``False`` if the player is dead (health == 0).

``BattlePlayer.attack(target)``
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Return type: ``None``

Prints the attack menu to be used against a target enemy.

Used in ``runBattle`` only. Otherwise, should not be called.

``BattlePlayer.takeDamage(amount)``
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Return type: ``int``

Makes the player take ``amount`` points of BASE damage.

The actual amount of damage taken will be scaled down (or up) based on ``BattlePlayer.nextDamageReduction`` .

Also decrements ``BattlePlayer.nextDamageReductionDuration`` if necessary.

Returns the actual amount of damage taken as an ``int`` .
