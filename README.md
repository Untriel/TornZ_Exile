## TornZ  
Is just the an addon with assets / models / animations.  
RyanZombies https://forums.bistudio.com/topic/182412-zombies-demons-36/  
Comatose Badger for this work on porting A2 Skins & some Zombie Skins & Hats :)  
Seperate Mod allows people to write a custom code without having to worry about existing initHandlers etc..  

## TornZ_Exile  
This contains the actually custom sqf/fsm code for the zombies themselves.  

<a href="https://www.paypal.com/cgi-bin/webscr?cmd=_s-xclick&hosted_button_id=2SUEFTGABTAM2"><img src="https://www.paypalobjects.com/en_US/i/btn/btn_donate_LG.gif" alt="[paypal]" />


#### Install Instructions  

1) Install TornZ & TornZ_Exile addons on your server  
Only available via A3Launcher http://a3launcher.com/  
2) Update your server fsm option in server_config_options  
3) Update your custom traders in exile_server_custom/traders/world  
4) PBO the exile_server_custom & add to to your server using -servermod=  
5) Merge the mpmission with your own mpmission file.  
6) You need to make loot tables for  
"zombies"  
"zombies_doctor"  
"zombies_police"  
"zombies_soldier"  


#### BattlEye Filters  
There are none, you will need to create your own for your server.  
Check out http://www.exilemod.com/topic/9708-battleye-filter-editor/  

#### Download TornZ & TornZ_Exile
Is only available via a3launcher @ http://a3launcher.com/  

#### Important
Max Player Count is around 70 players, due to limit on group size.  
It is possible to increase this by altering the group side (via SQF) if required.
