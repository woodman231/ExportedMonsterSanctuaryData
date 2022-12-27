# Exported Monster Sanctuary Data
This repository is simply the results of executing this Monster Sanctuary BepInEx [Plugin](https://github.com/woodman231/AddAndExportAllMonsters).

## A few notes about the plugin and how it might affect the data
- These objects have fields that are PascalCase and not camelCase like you might expect from typical JSON files.

- The plugin adds monsters to the player, sets the level of the monster to 42. Removes all equipment (the stick) and reset's the skills.

- Normalization and denomralization was hard to pick in some cases. Especially when avoiding reference loops. Sometimes only an ID is given, and sometimes additional details about the related data was given directly in the related field. If you feel like the wrong choices were made for normalized data and denomralized data, that is fine. Feel free to modify the source code of the Plugin and run it on your computer to suite your needs better. For a more clear example of what I mean see an Item and compare "UpgradesTo", and "UpgradeMaterials" properties. The "UpgradesTo" is just an item ID that references the next item; however, each "Item" in the "UpgradeMaterials" has the ID and all other properties associated with it.

- The game uses Unity Engine where a bunch of Classes are Associated with a Game Object. For example there are many Classes that are SubClasses of PassiveSkill. So for every passive skill I kept querying the game object aginst all possible classes in the assembly and anytime it got a non-null game object it got all of the fields from that class and declared that skill as IsClassType, and then ClassTypeProperties were the properties associated with that SubClass. For example Aazerach has the "Enlightened" skill (for light shift). The skill IsPassiveSkill = true, and then was determined to be of type PassiveEnlighten and thus the "IsPassiveEnlighten" = true, and has "PassiveEnlightenProperties" to give those details. This could prove to be helpful in the JavaScript truthy world because if you want to find all IsPassiveElnlightend, it will resolve to false if it is undefined, and will only return true if it is both defined and has the boolean value of true. So for example that same skill did not have the PassiveCurseChain class associated with it. Instead of "IsPassiveCurseChain" = false, just "IsPassiveCurseChain" and "PassiveCurseChainProperties" are undefined for that specific skill.

- Where possible descriptions were achieved by using a method on the class called GetToolTip, and then color codes, new lines and curly braces were removed.

- If the skill or item had some "On" methods those "On" method names were extracted and properties like "IsOnReceiveBuffPreCheck" = true were made possible. Again this will either only be defined as "IsOnReceiveBuffPreCheck" = true, or just simply not defined at all. There is no such thing as "IsOnReceiveBuffPreCheck" = false.

