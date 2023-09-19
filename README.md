# McKathlin Game Over Plugin for RPG Maker MZ

## Compatibility

This plugin is designed to play well by itself and with other plugins.
There are no known conflicts, but conflict is possible with other plugins
that directly alter Game Over behavior.

## Plugin Parameters

This plugin provides the following parameters:

* **Party Death Common Event ID** - number
* **Show Game Over Scene** - true/false
* **Reload Last Save** - true/false
* **After Game Over Common Event ID** - number

These parameters are explained in detail in the sections below.

### Party Death Common Event ID
Assigning a Party Death Common Event replaces the usual Game Over scene
call with a common event call. This lets you (the game designer) make 
something happen instead of (or before) the standard Game Over screen.
Open the database to Common Events to find the ID of the common event
to call on party death, and enter this ID number as the parameter.
Here are some pointers:
* The Party Death common event will run in the same scene where the party
  was just defeated. If the party died in battle, the common event will run
  in battle, right after the party-is-defeated message and music effect.
  So if you want to make something happen after a slow fadeout
  and return to the map, I recommend using the After Game Over Common Event
  instead.
* If you would like the Game Over screen (or a fadeout and cut to the map
  if "Show Game Over Scene" is false) to show at the end of your common
  event, remember to use the Game Over command in your common event.
  Avoiding the Game Over screen is easy: avoid calling the Game Over command.
* Directly calling the Game Over command from any event will still show
  the Game Over screen normally (unless you've set Show Game Over Scene to
  false). To force your custom party death behavior, use a command that
  calls your party death common event instead.

### Show Game Over Scene
RPG Maker's default behavior takes the player to Scene_GameOver on
party death or on a scene processing call to Scene_GameOver.
This processing shows a Game Over screen.
After the player sees the screen and presses any key,
the game exits to the title screen.

If Show Game Over Scene is set to false, player will see a fade to black
before going to the next scene. This will be the case regardless of whether
Game Over state is reached by party death or by a direct command in an event.

Whether or not the Game Over screen shows, which scene is next depends on
whether the After Game Over Common Event is set, and what it is set to.

### Reload Last Save
Set Reload Last Save to true to make the game automatically reload from its
most recent save on game over. If a Game Over Common Event is specified,
the reload occurs before the common event is reserved. If no common event
is given, then the reload happens instead of going to the title screen.

If Reload Last Save is true but the player has not yet saved,
then the player is returned to the Title Screen.

### After Game Over Common Event ID
Assigning a After Game Over common event makes gameplay continue after
the party loses, instead of RPG Maker's default behavior of returning the
party to the title screen. Open the database to Common Events to find the
ID of the common event to call on game over, and enter this ID number as
the parameter.

In the content of the common event, the game designer can customize what
happens when the party dies or reaches an event-dictated Game Over state.
The After Game Over common event might do some of the following things:
* Take away gold and/or items
* Return the player to a safe place
* Restore HP to one or more party members
* Have the party's rescuer say something
* ...anything that suits this game!

**IMPORTANT:** When control flows to the Game Over common event,
the screen will start blacked out. This gives the event time to handle
transfers and other processing before showing the player the screen.
Once those things are ready, **remember to fade in!**

The After Game Over Common Event (AGOCE) differs from the Party Death Common
Event (PDCE) in the following ways:
* The PDCE runs instead of or before the Game Over screen or fadeout;
  the AGOCE runs after the Game Over scene or fadeout completes.
* The PDCE only automatically replaces Game Overs caused by party death.
  The AGOCE autoruns after all Game Overs, regardless of their cause.
* The PDCE runs in the same scene where party death occurred.
  The AGOCE runs in a newly started map scene, with the screen faded to
  black, and the party leader revived to 1 HP.

## FAQ

**Q:** I've set a common event to occur after Game Over, but when I try it, the screen stays black and I can't see anything. Help!

**A:** After Game Over, the screen starts out faded to black to give the After Game Over Common Event a chance to handle transfers and other "behind-the-scenes" processing before showing the screen. To get rid of the blackness, make sure to run a "Fade In" command as part of your After Game Over Common Event.

**Q:** My game lets the player continue exactly where they left off after they die. But when they continue, the music for the map they're in doesn't autoplay like it's supposed to. Is this a bug, or do I need to do something differently?

**A:** This is not a bug. The silence gives the Game Over common event a chance to run its course without being interrupted by the BGM of the map where the player died. A transfer will cause the music of the destination to autoplay. Though you don't need the player to go anywhere, transferring the player to where they already are will fix your music problem. Here's how to do it:

1. At the start of your Game Over common event, use the Control Variables command
   three times to assign three variables: the player's current map ID, X coordinate,
   and Y coordinate.
2. Place a Player Transfer command at the part of the event when the map's BGM should
   start playing.
3. As always, remember to use a Fadein Screen command!
  (The fade that accompanies the Player Transfer command won't be sufficient.)

### For more help using the Game Over plugin, visit [Tyruswoo.com](https://www.tyruswoo.com/).

## Version History

**Version 1.0.0** - Jan 16, 2016
- Initial release of McKathlin Game Over for MV!

**Version 1.0.1** - Jan 16, 2016
- Minor edits to Version 1.0, mostly in help text.

**Version 1.0.2** - Jan 17, 2016
- Show/Skip Game Over Screen bugfix attempted, but found to cause the game to freeze on some machines.

**Version 1.0.3** - Feb 20, 2016
- Show/Skip Game Over Screen feature removed for now.

**Version 1.1.0** - Feb 26, 2016
- Show/Skip Game Over Screen feature is back. The bug appears to be fixed.

**Version 1.2.0** - Jun 15, 2016
- New parameter allows you to reload data from the player's last save.

**Version 1.3.0** - Aug 18, 2016
- Set the Party Death Common Event to customize what happens before (or instead of!) the Game Over fadeout.

**Version 1.3.1** - Sept 18, 2023
- This plugin is now free and open source under the [MIT License](https://opensource.org/license/mit/).