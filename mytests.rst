This file holds the tests. Remember to import the python file(s)
you wish to test, along with any other modules you may need.
Run your tests with "python3 ok -t --suite SUITE_NAME --case CASE_NAME -v"
--------------------------------------------------------------------------------

Suite 1

	>>> from ants import *

	Case 1	
		>>> import ants, importlib
		>>> hive = ants.Hive(ants.AssaultPlan())
		>>> dimensions = (2, 9)
		>>> colony = AntColony(None, hive, ants.ant_types(),
		...         ants.dry_layout, dimensions)
		>>> tunnel = [colony.places['tunnel_0_{0}'.format(i)]
		...         for i in range(9)]
	    >>> # Testing long effect stack
        >>> stun = StunThrower()
        >>> slow = SlowThrower()
        >>> bee = Bee(3)
        >>> colony.places["tunnel_0_0"].add_insect(stun)
        >>> colony.places["tunnel_0_1"].add_insect(slow)
        >>> colony.places["tunnel_0_5"].add_insect(bee)
        >>> for _ in range(3): # stun bee three times
        ...    stun.action(colony)
 
        >>> slow.action(colony) # slow bee once
		>>> stun.action(colony) # stun bee once more
		>>> slow.action(colony) # slow bee once more
 
        >>> colony.time = 0
        >>> bee.action(colony) # slowed (1/2), but even turn -> stunned (1/4)
        >>> bee.place.name
        'tunnel_0_5'
 
        >>> colony.time = 1
        >>> bee.action(colony) # still slowed (1/2), odd turn
        >>> bee.place.name
        'tunnel_0_5'
 
        >>> colony.time = 2
        >>> bee.action(colony) # slowed (2/2), but even turn -> stunned (2/4)
        >>> bee.place.name
        'tunnel_0_5'
 
        >>> colony.time = 3
        >>> bee.action(colony) # slowed twice (2/2)
        >>> bee.place.name
        'tunnel_0_5'
 
        >>> colony.time = 4
        >>> bee.action(colony) # slowed twice (2/2), even turn -> stunned (3/4)
        >>> bee.place.name
        'tunnel_0_5'
 
        >>> colony.time = 5
        >>> bee.action(colony) # stunned (4/4)
        >>> bee.place.name
        'tunnel_0_5'
 
        >>> colony.time = 6
        >>> bee.action(colony) # status effects have worn off
        >>> bee.place.name
        'tunnel_0_4'
		
        >>> colony.time = 7
        >>> bee.action(colony)
        >>> bee.place.name
        'tunnel_0_3'
		
        >>> colony.time = 8
        >>> bee.action(colony)
        >>> bee.place.name
        'tunnel_0_2'
 
        >>> colony.time = 9
        >>> bee.action(colony) 
        >>> bee.place.name
        'tunnel_0_1'
        >>> slow.armor
        1
		
        >>> colony.time = 10
        >>> bee.action(colony) 
        >>> bee.place.name
        'tunnel_0_1'
        >>> slow.armor
        0
	    
