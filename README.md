Thank you for using the Veilstone Gym pack, provided by SirMalo and Flo. Here are some instructions on how to use it:





### **How can I import this map into my own project?**



First, in Pokémon Studio, create a new map for your project and select "022 Veilstone Gym.tmx" (you can rename it as you want) in "Veilstone Gym Project/Data/Tiled/Maps". Update the maps, save in Studio, then launch your game to convert the map into RPG Maker XP.



Once it is done, close the game and go to "Veilstone Gym Project/Data" and copy "Map022.rxdata". Then paste it anywhere outside of your Project (on your Desktop for instance) and replace the map number in the filename by the number of your map in RPG Maker (to get it, select the newly converted map in RPG Maker and check the number on the bottom right of the interface). Finally, cut and paste the file in "\[Your Project]/Data" and replace the .rxdata file. This will add all the events in your map.



Don’t forget to edit the event no 30 "§ exit door" to change the warping command to the exterior of your Gym building.



You also need to edit the Maylene event on the map Exterior: copy the 4 script commands and paste them onto the event that will warp the player to the Gym (the door of the Gym, typically). Those lines turn OFF all the self-switches of all the sandbags and tires events, in order to reset the Gym when the player re-enters it.



Finally, don’t forget to copy the "00002 SandbagReset.rb" script from "Veilstone Gym Project/scripts" to the "scripts" folder of your project.





### **How does it work?**



The whole system of the puzzle isn’t that complex, but it requires a lot of command to handle all the animations for every position of each sandbag. Basically, each sandbag event has a page for each of its possible positions on the rails. Then, on each page, we check the direction the player is looking at and we play the corresponding animation.



There are 3 types of animations:

* The sandbag cannot move: it will only swing a little.
* The sandbag can move towards another stop on the rails: a “pre-animation” will play, then the sandbag will move towards the next stop, play a “stop animation” and a self-switch will turn ON to change the page of the event accordingly to the new bag position.
* The sandbag can move AND there is a tire pile next to the stop: the animation above will play but the event will check if the self-switch of the tire pile is ON or OFF. If it’s OFF, it will play a different sound effect and a breaking animation will play, then this self-switch will turn ON, deleting the tire pile.



As mentioned above, to make sure the player doesn’t get locked if they lose a battle or if they save then load the save, two other systems exist:

* Event no 35, located on the inaccessible ring on the bottom left, calls the script `Sandbag Reset` each time the map is loaded. This script “manually” checks which self-switch is ON for each sandbag and changes the position of those sandbags immediately. Without this script, the position of every sandbag would reset everytime the map would be reloaded, but their self-switches would remain the same, resulting in unwanted behaviours.
* The event that handles the warping to the Gym (in the demo, the Maylene character on the map "Exterior") also turns all the self-switches of each sandbag and tire event to OFF so the Gym puzzle is reset each time the player re-enters the map. If you forget it, it might result in some locking situations!



This Gym also features another system: half-tile stairs. When climbing onto the rings, the player will be raised by 16 pixels, or half a tile (because PSDK runs a 32x32 pixels grid). Unfortunately, the stairs systemtags cannot make this kind of slope for the moment, so we have to cheat a little and change the "offset\_screen\_y" of the player, the events on the platforms and their shadows “manually”. This happens in two ways:

* When the player touches the stairs while looking in the good direction, it will trigger a loop that will continually change their height (the `y` offset) until it reaches -16 (moving up) or 0 (moving down), and the same is done for their shadow.
* Event no 8, located on the ring at the bottom left of the map, automatically changes the characters that are on the rings or in the Champion platform, as well as their shadows, everytime the map is loaded.

Unfortunately, it’s a quite flimsy and **doesn’t work with following Pokémon**, so the warping event should disable followers for the Gym.







### **How can I make my own layout?**



The previous section should be clear enough to make you understand how everything works, but here are a few focal points if you want to create your own layout or puzzle with these sandbags:

* 1 page = 1 position, and each position requires multiples animations (immobile, moving and moving + breaking the tire pile). I think the official Gym has an example of each animation for every direction, so you just have to copy the good animation for the right direction and paste it in your own event.
* When there is a breakable tire pile, make sure the conditional branch looks for the correct event when it checks whether the pile was already destroyed or not, and make sure to turn ON the self-switch of that same event in the special conditional branch.
* Once you have your new layout completed, don’t forget to edit the script to enter manually every position for each self-switch of each sandbag, and don’t forget to edit the 4 script commands of your warping event to change the list of events that will have their self-switches turned OFF with the numbers of your new events for sandbags and tires.



**If you need help, please send a message in the dedicated thread in the Pokémon Workshop Discord Server.**

