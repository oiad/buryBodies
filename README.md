# [EPOCH 1.0.6.1] Bury and butcher bodies.
Bury and butcher bodies for epoch 1.0.6.1 by salival (https://github.com/oiad)

* Discussion URL: to be updated.

* Tested as working on a blank Epoch 1.0.6.1 server
* Contains a server sided scheduler module to clean up old weapon crates, graves and crosses after 25 minutes.
* Removes the flies sound when body is buried for performance.

# Install:

* This install basically assumes you have NO custom variables.sqf or compiles.sqf or fn_selfActions.sqf, I would recommend diffmerging where possible.

**[>> Download <<] (https://github.com/oiad/buryBodies/archive/master.zip)**

# Mission folder install:

1. In mission\init.sqf find: <code>call compile preprocessFileLineNumbers "\z\addons\dayz_code\init\variables.sqf";</code> and add directly below:

	```sqf
	call compile preprocessFileLineNumbers "dayz_code\init\variables.sqf";
	```
	
2. In mission\init.sqf find: <code>call compile preprocessFileLineNumbers "\z\addons\dayz_code\init\compiles.sqf";</code> and add directly below:

	```sqf
	call compile preprocessFileLineNumbers "dayz_code\init\compiles.sqf";
	```

# dayz_server install:

1. Overwrite the following files to your dayz_server folder preserving the directory structure:
	```sqf
	dayz_server\system\scheduler\sched_corpses.sqf
	dayz_server\system\scheduler\sched_init.sqf
	dayz_server\system\scheduler\sched_lootCrates.sqf
	```

# Battleye CreateVehicle.txt filter:

1. In your config\<yourServerName>\Battleye\createvehicle.txt around line 2 find: <code>5 !(^DZ_|^z_|^pz_|^WeaponHolder|Box|dog|PZombie_VB|^Smoke|^Chem|^._40mm|_DZ$|^Trap)</code> add this to the end of the line:

	```sqf
	!=GraveCross1
	```
	
	So it will then look like this for example:

	```sqf
	5 !(^DZ_|^z_|^pz_|^WeaponHolder|Box|dog|PZombie_VB|^Smoke|^Chem|^._40mm|_DZ$|^Trap) <CUT> !=GraveCross1
	```