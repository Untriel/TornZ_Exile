## TornZ  
Is just the an addon with assets / models / animations.  
RyanZombies https://forums.bistudio.com/topic/182412-zombies-demons-36/  
Comatose Badger for this work on porting A2 Skins & some Zombie Skins & Hats :)  
Seperate Mod allows people to write a custom code without having to worry about existing initHandlers etc..  

## TornZ_Exile  
This contains the actually custom sqf/fsm code for the zombies themselves.  

<a href="https://www.paypal.com/cgi-bin/webscr?cmd=_s-xclick&hosted_button_id=2SUEFTGABTAM2"><img src="https://www.paypalobjects.com/en_US/i/btn/btn_donate_LG.gif" alt="[paypal]" />


#### Install Instructions  

1) Install TornZ & TornZ_Exile addons on your server<br />
Download the addons (via A3Launcher) and upload them to your server. It should now look like this:<br />
server/@TornZ<br />
server/@TornZ_Exile<br />

2) Add the parameters to your server start script/batch:<br />
-mod=@TornZ;@TornZ_Exile -servermod=@exile_server_custom

3) Download the loottable compiler from exilemod.com and open LootTables.h, at the bottom add (you can change these settings, just keep the classes):
```
///////////////////////////////////////////////////////////////////////////////
// TornZ + Tornz_Exile
///////////////////////////////////////////////////////////////////////////////
>zombies
80, Trash
20, MedicalItems

>zombies_doctor
80, Trash
20, MedicalItems

>zombies_police
80, Trash
20, MedicalItems

>zombies_soldier
80, Trash
20, MedicalItems
```

Save and open compile.bat.

4) Download "exile_server_config.pbo" from server/@exileserver/addons and unpack it. In config.cpp:
Find:
serverFSM = "exile_server\fsm\main.fsm";

Replace with:
serverFSM = "exile_server_custom\fsm\main.fsm";

Find:
class CfgLootTables
{

...........

};

Replace with:
The content from CfgLootTables.hpp (lootcompiler).

Save, repack the pbo and upload it.

5) If you don't have any custom traders on your server, you can turn the folder "exile_server_custom" into pbo. Create the following folders on your server:
server/@exile_server_custom/addons/

Place the pbo in the addons folder.

6) Unpack Exile.Altis.pbo and open config.cpp
Find:
class CfgExileCustomCode 
{
    /*
        You can overwrite every single file of our code without touching it.
        To do that, add the function name you want to overwrite plus the 
        path to your custom file here. If you wonder how this works, have a
        look at our bootstrap/fn_preInit.sqf function.

        Simply add the following scheme here:

        <Function Name of Exile> = "<New File Name>";

        Example:

        ExileClient_util_fusRoDah = "myaddon\myfunction.sqf";
    */

};

Replace with:
class CfgExileCustomCode 
{
    /*
        You can overwrite every single file of our code without touching it.
        To do that, add the function name you want to overwrite plus the 
        path to your custom file here. If you wonder how this works, have a
        look at our bootstrap/fn_preInit.sqf function.

        Simply add the following scheme here:

        <Function Name of Exile> = "<New File Name>";

        Example:

        ExileClient_util_fusRoDah = "myaddon\myfunction.sqf";
    */

    // Changes Required by TornZ & TornZ_Exile

    ExileServer_object_construction_network_buildTerritoryRequest = "exile_server_custom\override\ExileServer_object_construction_network_buildTerritoryRequest.sqf";

    ExileServer_system_lootManager_initialize = "exile_server_custom\override\ExileServer_system_lootManager_initialize.sqf";
    ExileServer_system_lootManager_spawnLootForPlayer = "exile_server_custom\override\ExileServer_system_lootManager_spawnLootForPlayer.sqf";
    ExileServer_system_lootManager_thread_spawnLoot = "exile_server_custom\override\ExileServer_system_lootManager_thread_spawnLoot.sqf";
    ExileServer_system_process_postInit = "exile_server_custom\override\ExileServer_system_process_postInit.sqf";
    ExileServer_system_territory_database_load = "exile_server_custom\override\ExileServer_system_territory_database_load.sqf";

    ExileClient_object_trader_create = "override\ExileClient_object_trader_create.sqf";

};

7) Open description.ext
Find:
class CfgRemoteExec
{
    class Functions
    {
        mode = 1;
        jip = 0;
        class ExileServer_system_network_dispatchIncomingMessage     { allowedTargets=2; };
    };
    class Commands
    {
        mode=0;
        jip=0;
    };
};

Replace with:
class CfgRemoteExec
{
    class Functions
    {
        mode = 1;
        jip = 0;
        class fnc_AdminReq { allowedTargets=2; };
        class ExileServer_system_network_dispatchIncomingMessage { allowedTargets=2; };
        class TornZ_fnc_re_z_initAll { allowedTargets=1; };
        class TornZ_fnc_re_z_attack { allowedTargets=1; };
    };
    class Commands
    {
        mode=0;
        jip=0;
    };
};

8) Open initPlayerLocal.sqf
Find:
if (!hasInterface || isServer) exitWith {};

Replace with:
if (!hasInterface || isServer) exitWith {};

// Custom Traders
waitUntil {!isNil 'ExileServerCustom_Traders'};
{
    _x call ExileClient_object_trader_create;
} forEach (call ExileServerCustom_Traders);

Repack Exile.Altis.pbo, upload and you should be set.

#### BattlEye Filters  
There are none, you will need to create your own for your server.  
Check out http://www.exilemod.com/topic/9708-battleye-filter-editor/  

#### Download TornZ & TornZ_Exile
Is only available via a3launcher @ http://a3launcher.com/  

#### Important
Max Player Count is around 70 players, due to limit on group size.  
It is possible to increase this by altering the group side (via SQF) if required.
