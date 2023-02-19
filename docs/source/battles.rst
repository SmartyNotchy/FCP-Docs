How Battles Run
===============

.. _Running Battles:

runBattle
------------

Battles in the game are started with the ``runBattle`` function, which has the following method header:

.. code-block:: py

   def runBattle(player, enemy):

where
   - player is a ``BattlePlayer`` object
   - enemy is a ``BattleEnemy`` object or any subclass of ``BattleEnemy``

As such, battles are ONLY influenced by the state of the player & enemy, and nothing else.

``runBattle`` returns a boolean based on the result of the battle.
If the player wins, ``runBattle`` returns ``True``. 
Otherwise, if the enemy wins, ``runBattle`` returns ``False``.
