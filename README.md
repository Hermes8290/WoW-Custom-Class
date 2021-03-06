# WoW-Custom-Class

## Creating a custom class ##

This repository documents my findings when creating a custom class for WoW. I am doing this using TrinityCore 3.3.5a.

Initially I will be dumping information on here without documenting it properly, however I will periodically revisit this and clean it up.

## 2019-01-13 ##

My initial goal is to get the shell of a class in. Once I have that process fully figured out I will start adding more specific features for my class (Engineer).

The first file to modify is ChrClasses.dbc. I have added a new row using MyDBCEditor:

https://wowdev.wiki/DB/ChrClasses
![ChrClasses.dbc modified with MyDBCEditor](https://i.imgur.com/KYF1c8X.png)

Next we need to modify CharBaseInfo.dbc which maps which races are allowed to play which classes. I have made warrior able to play Engineer. MyDBCEditor and DBCUtil didn't seem to be able to open this file for me, but Tallis was able to.

https://wowdev.wiki/DB/CharBaseInfo
![CharBaseInfo.dbc modified with Tallis](https://i.imgur.com/1Feg8Xm.png)

At this stage you will be getting this error in the Character Create screen:

![Char Creation Screen Error](https://i.imgur.com/PmTWSJQ.png)

To resolve this we need to add a new button for the new class in Interface\GlueXML\CharacterCreate.xml. We also need to modify the CharacterCreate.lua file in the same directory to add the texcoord offset for "ENGINEER" and increment the MAX_CLASSES_PER_RACE variable. Also add all the strings needed for engineer in GlueStrings.lua, also within the GlueXML directory. You can search 'WARRIOR' and copy all these strings renaming them.

I also modified Interface\GLUES\CHARACTERCREATE\UI-CHARACTERCREATE-CLASSES.blp to add the new icon. This is what the texcoord offset targets.

Now we can see our class in the character creation screen:

![Char Creation Screen With Engineer](https://i.imgur.com/rV7WT36.jpg)

Now I add the basic server side database data to get the character created. I only add data for level 1.

```sql
INSERT INTO playercreateinfo (race, class, map, zone, position_x, position_y, position_z, orientation) VALUES (1, 12, 0, 12, -8949.95, -132.493, 83.5312, 0);
INSERT INTO player_levelstats (race, class, LEVEL, str, agi, sta, inte, spi) VALUES (1, 12, 1, 17, 20, 21, 23, 25);
INSERT INTO player_classlevelstats (class, LEVEL, basehp, basemana) VALUES (12, 1, 20, 0);
```

Trying to create the class now gives you this error:

![Error when creating class](https://i.imgur.com/BTkuW2S.png)

So we need to update the emulator code to handle the new class. Luckily this is quite simple:

![Emulator code modification for new class](https://i.imgur.com/A1oNId4.png)

We can now login with the new custom class but when you open the talent or character panel the client crashes. This is because we are missing gt DBC data for the new class.

I wrote a little program to automate adding this data to the DBC files. Doing it manually would have been a slow tedious task and very error prone. I have included this program in this repository.

![Screenshot of program code for adding gt DBC data](https://i.imgur.com/1H3FjcP.png)

Now we can open the character panel without the client crashing:

![Character panel open without client crash](https://i.imgur.com/a46e3BW.png)


