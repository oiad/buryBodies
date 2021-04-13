# [EPOCH 1.0.7] Bury and butcher bodies.
Bury and butcher bodies for epoch 1.0.7 by salival updated by Airwaves Man (https://github.com/oiad)

* Discussion URL: https://epochmod.com/forum/topic/43501-release-bury-and-butcher-body-for-1061/

* Tested as working on a blank Epoch 1.0.7 server
* Contains a server side scheduler module to clean up old weapon crates, graves and crosses after 25 minutes.
* Removes the flies sound when body is buried for performance.

# REPORTING ERRORS/PROBLEMS

1. Please, if you report issues can you please attach (on pastebin or similar) your CLIENT rpt log file as this helps find out the errors very quickly. To find this logfile:

	```sqf
	C:\users\<YOUR WINDOWS USERNAME>\AppData\Local\Arma 2 OA\ArmA2OA.RPT
	```

**[>> Download <<](https://github.com/oiad/buryBodies/archive/master.zip)**

# Install:

* This install basically assumes you have a custom variables.sqf, compiles.sqf and fn_selfActions.sqf.

# Mission folder install:

1. 	Open your fn_selfactions.sqf and search for:

	```sqf
	// ZSC
	if (Z_singleCurrency) then {
	```

	Add this code lines above:
	
	```sqf	
	if (!_isAlive && _isMan && !_isZombie && {!(_cursorTarget isKindOf "Animal")}) then {
		local _isButchered = _cursorTarget getVariable ["bodyButchered",false];
		if (!_isButchered) then {
			if ("ItemEtool" in _itemsPlayer) then {
				if (s_player_bury_human < 0) then {
					s_player_bury_human = player addAction [format["<t color='#0059FF'>%1</t>",localize "STR_CL_BA_BURY"],"scripts\buryActions.sqf",[_cursorTarget,"bury"],0,false,true];
				};
			} else {
				player removeAction s_player_bury_human;
				s_player_bury_human = -1;
			};
			if ({_x in ["ItemKnife","ItemKnife5","ItemKnife4","ItemKnife3","ItemKnife2","ItemKnife1"]} count _itemsPlayer > 0) then {
				if (s_player_butcher_human < 0) then {
					s_player_butcher_human = player addAction [format["<t color='#0059FF'>%1</t>",localize "STR_CL_BA_BUTCHER"],"scripts\buryActions.sqf",[_cursorTarget,"butcher"],0,false,true];
				};
			} else {
				player removeAction s_player_butcher_human;
				s_player_butcher_human = -1;
			};
		};
	};
	```	
2. In fn_selfactions search for this codeblock:

	```sqf
	player removeAction s_bank_dialog3;
	s_bank_dialog3 = -1;
	player removeAction s_player_checkWallet;
	s_player_checkWallet = -1;	
	```	
	And add this below:
	
	```sqf
	player removeAction s_player_bury_human;
	s_player_bury_human = -1;
	player removeAction s_player_butcher_human;
	s_player_butcher_human = -1;	
	```
3. Open your variables.sqf and search for:

	```sqf
	s_bank_dialog3 = -1;
	s_player_checkWallet = -1;	
	```
	And add this below:
	
	```sqf
	s_player_bury_human = -1;
	s_player_butcher_human = -1;	
	```	
	
# dayz_server install:

1. Overwrite the following files to your dayz_server folder preserving the directory structure:
	```sqf
	dayz_server\system\scheduler\sched_corpses.sqf
	dayz_server\system\scheduler\sched_init.sqf
	dayz_server\system\scheduler\sched_lootCrates.sqf
	```
# Battleye Script.txt filter:

1. In your config\<yourServerName>\Battleye\scripts.txt around line 3: <code>5 addBackpack</code> add this to the end of it:

	```sqf
	!="forEachIndex]} forEach (_backPackMag select 0);};\n\n_crate addBackpackCargoGlobal [_backPack,1];\n};\n\n_sound = _corpse getVariable"
	```

	So it will then look like this for example:

	```sqf
	5 addBackpack <CUT> !="forEachIndex]} forEach (_backPackMag select 0);};\n\n_crate addBackpackCargoGlobal [_backPack,1];\n};\n\n_sound = _corpse getVariable"
	```
	
2. In your config\<yourServerName>\Battleye\scripts.txt around line 5: <code>5 addWeapon</code> add this to the end of it:

	```sqf
	!="ect 2];\n_cross setVariable [\"bury\",true,true];\n};\n\n{_crate addWeaponCargoGlobal [_x,1]} forEach weapons _corpse;\n{_crate addMaga"
	```

	So it will then look like this for example:

	```sqf
	5 addWeapon <CUT> !="ect 2];\n_cross setVariable [\"bury\",true,true];\n};\n\n{_crate addWeaponCargoGlobal [_x,1]} forEach weapons _corpse;\n{_crate addMaga"
	```	
	
3. In your config\<yourServerName>\Battleye\scripts.txt around line 48: <code>1 humanity</code> add this to the end of it:

	```sqf
	!="ckMag\",\"_backPackWpn\",\"_crate\",\"_corpse\",\"_cross\",\"_gain\",\"_humanityAmount\",\"_isBury\",\"_grave\",\"_name\",\"_playerNear\",\"_backPack\""
	```

	So it will then look like this for example:

	```sqf
	1 humanity <CUT> !="ckMag\",\"_backPackWpn\",\"_crate\",\"_corpse\",\"_cross\",\"_gain\",\"_humanityAmount\",\"_isBury\",\"_grave\",\"_name\",\"_playerNear\",\"_backPack\""
	```	
	
4. In your config\<yourServerName>\Battleye\scripts.txt around line 53: <code>1 nearEntities</code> add this to the end of it:

	```sqf
	!="erNear = {isPlayer _x} count (([_corpse] call FNC_GetPos) nearEntities [\"CAManBase\", 10]) > 1;\nif (_playerNear) exitWith {dayz_a"
	```

	So it will then look like this for example:

	```sqf
	1 nearEntities <CUT> !="erNear = {isPlayer _x} count (([_corpse] call FNC_GetPos) nearEntities [\"CAManBase\", 10]) > 1;\nif (_playerNear) exitWith {dayz_a"
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
	
**** *For Epoch 1.0.6.2 only* ****
**[>> Download <<](https://github.com/oiad/buryBodies/archive/refs/tags/Epoch_1.0.6.2.zip)**

Visit this link: https://github.com/oiad/buryBodies/tree/Epoch_1.0.6.2	