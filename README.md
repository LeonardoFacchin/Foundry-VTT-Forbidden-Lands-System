## Foundry VTT - Forbidden Lands System

**version**: 0.2.0  
**supported browsers**: Chrome, Edge  
(NB: Firefox won't display character sheets as intended. Right now I'm inclined not to bother trying to make the system work with Firefox, mostly because on my PC Firefox performance was way worse than Edge's or Chrome's. If enough people are **really really** interested I might change my mind, but I suggest using Chrome to be on the safe side)

# New features and bug fixes in v0.2
- Implemented a custom Combat Tracker (more on this below).
- Added support for the "Pack Rat" talent. The talent is now taken into account when computing encumbrance. When hovering over the encumbrance meter on the character sheet a tooltip will show the current encumbrance value vs the maximum carrying capacity of the character.
- fixed a bug preventing players from being able to use NPC spells from the NPC sheet.
- fixed a few bugs related to macro creation by dragging and dropping spells onto the macro bar.

# Features not yet supported
- No Stronghold sheet, nor functions
- No Hidden Combinations advanced combat
- No way to track Poisoned or Diseased state on the character sheet (use tokens state as a stopgap, please)

# Supported features and mechanics
- Sheets for the three actor types: PCs, NPCs and Monsters
- Sheets for:
    - Monster Attacks, Monster Special Abilities
    - Talents, Spells, Critical Injuries
    - Weapons, Armor, Equipment

The system supports dragging and dropping items onto the actor sheet.
The advantage is that once the required items have been created, very little typing will be required while running sessions. If a character needs a sword, the user will just have to drag the sword from the World Items or Compendium panels and the sword will be added to that character's inventory. Same goes for Equipment, Talents, Injuries, etc.  
The downside is the frontloading of the import process: all the items required to run a session will have to be imported from the rulebook in advance or created on the spot by a user with permission. I can't include the items in the system as Compendium packs because of copyright.

# Automation 
Most dice checks a user will be required to roll during the game are automated to some extent.
As a general rule, if an element on the actor sheet gets highlighted in orange when hovering, then that element can be interacted with according to the following guidelines:
- Double clicking an attribute, skill, weapon or equipment name will roll the corresponding check, automatically retrieving all the required informations. For example, if double clicking a weapon's name will cause an attack check to be rolled by adding together the character's relevant current attribute value, skill and the current gear bonus of the weapon, including artifact dice modifiers.
Injuries and Talents effects are **NOT** automatically included in the computation. Character's and NPC's sheets have a modifier track that allows you to add a modifier to the roll (range: -6 to + 6) and up to 3 d8 artifact dice (to account for Talents, for example).
All these rolls support Pushing (as many times as necessary) and rolling the Pride Die. This can be done by clicking on the relevant buttons on the message displayed in the chat log. Rolling the Pride Die will automatically prevent further Pushing.

- Double clicking a spell name will pop up a dialog that will let the player enter relevant informations before casting the spell, like the number of Willpower points spent or if ingredients or a grimoire/scroll are being used.
If the spellcasting talent rank has been configured in the talents tab, the system will automatically compute the correct number of Willpower points the player can spend according to spell vs talent rank, including the effect of a grimoire/scroll. The mishap chance is displayed to help making an informed decision.

- Double clicking the name of an Armor piece will automatically roll an Armor check by adding together the armor values of the top-most Body armor and the top-most Head armor appearing on the Armor Tab on the character sheet. The order Armor pieces are displayed on the sheet can be re-arranged by dragging and dropping the items on the sheet.

- Hovering the mouse over an element name will cause a tooltip with additional information to show if/when relevant (for example, injury effects or the kind of check that a specific item will add a bonus die to can be displayed this way without cluttering the sheet). The tooltip text is configured in the item creation panel

- Double clicking an item icon (when interactable) will usually open the corresponding item sheet, allowing the user to modify the item (let's say a player character's Longsword has been damaged by one point and the Crafting roll to repair it fails. The player can double click the Longsword icon in the inventory and change the name of the item to "Worn Longsword" and its bonus to 1 instead of 2).

- Critical Injuries timers are automatically rolled when the injury is added to the character sheet. An injury can exist in one of three states:
    - Lethal: it's an injury that will kill the character if not treated. In order to make it stand out it's displayed with a full red background and a crossed bones skull icon on the sheet.
    - Open: a non lethal injury that has not been treated yet. A treated lethal injury will turn into an Open injury by double clicking its name
    - Healing: a treated injury. A treated Open injury can be turned into a Healing injury by double clicking its name. The healing time is automatically halved (rounded up) when an injury state is set to healing

- Double clicking the "plus" or "minus" icons next to any Consumable will increase or decrease its Die size.  
Double clicking a Cosnumable Die icon will roll a Consumable check and post its result to the chat. If the check is failed the die size will automatically be reduced one step.

- Double clicking a monster attack die icon will roll the corresponding attack and post it to the chat together with its description.  
Double clicking the "Monster Attack" title on the Monster sheet will randomly select a monster attack, roll it and post the result to the chat.

- Double clicking a Monster Armor name will roll an Armor check for the monster

- Dragging and Dropping an Attribute, Skill, Weapon, Equipment or Spell element from the character sheet to the macro bar will automatically add a macro that will perform the corresponding check once clicked.  
NB: a token must be selected for the macro to actually work. An error message will be displayed asking the player to select a single token if he didn't.

- To roll a Forbidden Land style dice-check for which no automation has been built, type the following command in the chat window:  
`/fbl(nAtt, nSkill, nGear, artifactDieOne, artifactDieTwo,...)`  
For example `/fbl(4, -1, 2, d10)` would roll 4 Base Dice, 1 negative Skill Die, 2 Gear Dice and a single d10 Artifact Die. This roll doesn't support pushing (yet?)

- Spells can't be added to the Character sheet directly. They are automatically added when a Spellcasting talent is dragged onto the character sheet and automatically removed when the Talent is removed.  
Make sure to correctly import the talents and spells by configuring the right magical discipline associated to each of them.

# Combat Tracker
![](https://github.com/LeonardoFacchin/Foundry-VTT-Forbidden-Lands-System/blob/media/readme_images/CombatTracker_Illustration.png?raw=true)

This custom implementation of the Foundry VTT default Combat Tracker should make it somewhat easier to track combatants' actions and initiative order according to the Forbidden Lands game rules.  

Features:
- tracking of fast and slow actions expenditure. Click on the "F" and "S" icons to toggle their status between "available" and "spent". All the combatants' actions are automatically reset to "available" when a new round is started.
- The "Lightning Fast" talent is automatically detected and accounted for by the initiative algorithm
- Actors can be flagged for eligibility of Surprise bonus (see the picture to learn which icon to click in order to activate the bonus).
- An actor that is allowed to act twice in a turn can be flagged by clicking the "Forward" icon **before** rolling for initiative. The combatant will automatically be duplicated and cards drawn for his second action.
- If the fight includes 10 or less combatants, each one will receive his own initiative card, otherwise combatants will be grouped according to the Forbidden Lands rules and each group will act on the same initiative card.  

Limitations:
- In cases where a combatant can draw more than one initiative card, the system will automatically assign that combatant the lowest initiative score possible (i.e. no choosing your own card for speed and simplicity. This is the greatest departure from the "rules as written" but I think is a fair compromise)
- When more than ten combatants are present, in order for a group of combatants to benefit from the Surprise bonus, **all** of the combatants in the group should be marked accordingly. Otherwise the behaviour will be inconsistent: the group initiative will be set by the first combatant processed by the algorithm among those belonging to the group (a group of combatants is based on the actor the tokens was created from, even if unlinked).
- Because of Foundry VTT managing of permissions, only players with a permission level of Assistant GM or higher will be able to mark their actions or activate any icons on the tracker. The GM will have to assign the permissions or manage the display of the action economy him/herself (once I get a grasp of sockets I may try to remove this limitation).


# Feedback 
Feedback is very welcome. Please report any bug and I will try to look into it as soon as possibile.  
Up to about the first week of april 2020 I didn't know anything about JS, HTML or CSS, except that they existed. So, I'm a noob at software development. That means the code behind this system will hardly be optimized. It's much more likely to be a monster of a spaghetti code mess. Sorry about that.

# Credits
Thanks to:
- Free League Publishing for creating a great RPG
- Atropos for creating the outstanding Foundry VTT and building such a tightly-knit community
- Game-icons.net for all the great svg art and icons
- The Discord Foundry VTT community members. Without their help I wouldn't have been able to put this system together
- senutnareph, for creating the high quality dice icons and sharing them with the community ( https://www.reddit.com/r/ForbiddenLands/comments/amyzeq/forbidden_lands_dice_icons_for_roll20/ )
- eldersign for his precious feedback and his willingness to playtest this system.

# Media
![](https://github.com/LeonardoFacchin/Foundry-VTT-Forbidden-Lands-System/blob/media/readme_images/ChacracterSheet2020-07-12_104534.png?raw=true)
![](https://github.com/LeonardoFacchin/Foundry-VTT-Forbidden-Lands-System/blob/media/readme_images/Monster_Sheet_2020-07-12_104739.png?raw=true)
![](https://github.com/LeonardoFacchin/Foundry-VTT-Forbidden-Lands-System/blob/media/readme_images/NPC_Sheet_2020-07-12_105043.png?raw=true)
![](https://github.com/LeonardoFacchin/Foundry-VTT-Forbidden-Lands-System/blob/media/readme_images/Melee_Check_Pushed_2020-07-12_105314.png?raw=true)
![](https://github.com/LeonardoFacchin/Foundry-VTT-Forbidden-Lands-System/blob/media/readme_images/Monster_Attack_Check2020-07-12_105520.png?raw=true)
![](https://github.com/LeonardoFacchin/Foundry-VTT-Forbidden-Lands-System/blob/media/readme_images/Spell_Casting_2020-07-12_105139.png?raw=true)
![](https://github.com/LeonardoFacchin/Foundry-VTT-Forbidden-Lands-System/blob/media/readme_images/Spell_Casting_Check_Annotation_2020-07-12_105635.png?raw=true)