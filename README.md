# Abyssal Reliquary: The Fall of Ashen Veil
## Official RPG Terminal MMO

**Abyssal Reliquary: The Fall of Ashen Veil** is a dark-fantasy RPG terminal MMO prototype built in Python. It combines a command-line RPG console with a Qt sidecar interface, a local server, account login, persistent shared world state, save/load, audio playback, an asset scanner, and an open-world realm-walking layer.

The current build is a **V5.3 Release Candidate** intended for local testing, LAN-style experimentation, and GitHub development.

Inspired by the loot-grind and class-building feel of Diablo-like ARPGs, and the ruined-world atmosphere, hostile lore, and oppressive boss tone of Dark Souls-like fantasy. All game names, realms, bosses, items, lore, and mechanics in this project are original to ARTFOAV.

## Table of Contents
1. [What This Game Is](#what-this-game-is)
2. [Current Build Status](#current-build-status)
3. [Project Architecture](#project-architecture)
4. [First Run](#first-run)
5. [Recommended Launch](#recommended-launch)
6. [Build EXE](#build-exe)
7. [Game Lore](#game-lore)
8. [World / Realms](#world--realms)
9. [Classes and Character Creation](#classes-and-character-creation)
10. [Combat and Progression](#combat-and-progression)
11. [Loot, Gear, Rarities, and Affixes](#loot-gear-rarities-and-affixes)
12. [Pets and Followers](#pets-and-followers)
13. [NPCs, Quests, Economy, and Auction Houses](#npcs-quests-economy-and-auction-houses)
14. [Open World Realm Walking](#open-world-realm-walking)
15. [Local Server and Login](#local-server-and-login)
16. [Save / Load](#save--load)
17. [Audio and Asset System](#audio-and-asset-system)
18. [Settings](#settings)
19. [Folder Structure](#folder-structure)
20. [Tester Notes](#tester-notes)
21. [Known Limits](#known-limits)
22. [Development Roadmap](#development-roadmap)

## What This Game Is

ARTFOAV is a **dark fantasy terminal RPG** with a split interface:
CMD / Terminal Console = typing, menus, prompts, combat log, choices
Qt Sidecar UI         = map, player model, status lights, quest tracker, gear, monster status
Local Server          = shared world state, accounts, login tokens, session persistence
Audio Engine          = looping music and event-driven sound hooks

This is not an Electron app. It is a Python game that runs through the terminal and a Python Qt sidecar UI.

The game is moving from strict dungeon crawler toward an **8-bit terminal MMO-style open world**, while preserving the command-driven retro RPG identity.

## Current Build Status
**Build:** V5.3 Release Candidate  
**Primary language:** Python  
**UI:** PySide6 / Qt sidecar  
**Terminal color:** colorama-compatible ANSI output  
**Server:** localhost TCP JSON packet exchange  
**Audio:** dedicated Python audio engine using Windows MediaPlayer fallback and pygame fallback  
**Target platform:** Windows first, Python source runnable elsewhere with dependency adjustments  

What is real in this build:
- terminal RPG gameplay loop
- character creation
- save/load
- old save import
- server login/register
- local TCP server
- shared server world state
- Qt sidecar UI
- audio engine
- asset scanner
- settings menu
- save validation
- realm walking concept
- dungeon exploration
- turn-based combat
- loot generation
- rarity stars/colors
- pets/followers framework
- lore codex
- quest tracker display

What is not finished yet:
- full synchronized multiplayer combat
- remote/public internet hosting security
- finished MMO zone simulation
- full item database persistence for every generated item outside saves
- finished skill tree UI
- full crafting material inventory
- final balance pass

## Project Architecture
Game Console
    artfoav_terminal_v5_bridge.py
    Owns gameplay, input, combat, saves, menus, and live-state writing.

Qt Sidecar UI
    artfoav_sidecar_ui.py
    Reads runtime/live_state.json and displays status/map/player/monster info.

Launcher
    artfoav_launcher.py
    Starts server/audio/UI/console depending on selected BAT.

Audio Engine
    artfoav_audio_engine.py
    Plays looping soundtrack and watches runtime/audio_events_queue.jsonl.

Asset Indexer
    artfoav_asset_indexer.py
    Scans custom/assets sound folders and writes runtime/asset_manifest.json.

Local Server
    artfoav_local_server.py
    Hosts local shared world state and account/session auth.

Server Client
    artfoav_server_client.py
    Manual testing tool for server packets.

Runtime flow:
Game Console
    ↓ writes
runtime/live_state.json
    ↓ watched by
Qt Sidecar UI

Game Console
    ↓ pushes authenticated player state
Local Server
    ↓ writes
server_worlds/default/world_state.json

Game Console
    ↓ queues
runtime/audio_events_queue.jsonl
    ↓ watched by
Audio Engine

## First Run

Install Python 3.10+ first.

Install dependencies:
python -m pip install -r requirements.txt

Recommended full test run:
RUN_GAME_WITH_UI_AND_SERVER.bat

This starts:

1. local server
2. audio engine
3. Qt sidecar UI
4. CMD game console

At the title menu:
Server login / register

Use this flow:
1. Register account
2. Login
3. Create or load character
4. Enter town hub
5. Open world map / Walk realms
6. Enter dungeon gate or travel between realms

## Recommended Launch

### Full recommended source launch
RUN_GAME_WITH_UI_AND_SERVER.bat

### Game + UI without server

RUN_GAME_WITH_UI.bat

### Server only
RUN_LOCAL_SERVER.bat

### Server client test tool
RUN_SERVER_CLIENT.bat


### Audio engine only
RUN_AUDIO_ONLY.bat

### Asset indexer
RUN_ASSET_INDEXER.bat

## Build EXE

Build all EXEs:
BUILD_EXE_WINDOWS.bat

Expected EXEs:
ARTFOAV_Launcher.exe
ARTFOAV_GameConsole.exe
ARTFOAV_SidecarUI.exe
ARTFOAV_AudioEngine.exe
ARTFOAV_LocalServer.exe
ARTFOAV_ServerClient.exe

Recommended built launch:
dist\RUN_EXE_WITH_UI_AND_SERVER.bat

EXE layout:
dist/
├── ARTFOAV_Launcher.exe
├── ARTFOAV_GameConsole.exe
├── ARTFOAV_SidecarUI.exe
├── ARTFOAV_AudioEngine.exe
├── ARTFOAV_LocalServer.exe
├── ARTFOAV_ServerClient.exe
├── runtime/
├── saves/
├── server_worlds/
├── config/
├── assets/
└── custom/

## Game Lore
The world is veiled in ash, brimstone, black wax, saint-bone, cursed gold, marrow, and chains.

At the center of the story is the **Abyssal Reliquary**, a cursed cathedral-prison built to contain the unborn heart of a demon too vast to enter the world in physical form.

The prison failed.

It did not simply contain the demon.

It became an incubation chamber.

The demon dreams itself into existence through cursed relics scattered across eight realms. Every item of power is suspect:
Weapons are its teeth.
Armor is its ribs.
Coins are its scales.
Potions remember its blood.
Relics are pieces of its unborn soul.

The player enters the Reliquary for survival, power, answers, or greed. But every victory may feed the thing below.

The core horror is not just that the dungeon kills heroes. It dresses them in cursed strength, lets them win, then uses that strength as a body.

## The First Relic
The First Relic is not a normal artifact.

It is an egg.

It is the unborn demon heart trying to become real through objects, gold, armor, weapons, blood, and ambition.

At the end of the main path, the player faces:
Bergus The Satanic Arsonist
    ↓
The Relic-Worn Self
    ↓
The Unborn Below
    ↓
Seal / Wear / Break the Egg

The strongest canon postgame route is:
Break the Egg

This scatters the demon into the eight realms, awakens world bosses, and fully opens Octa-level progression.

## World / Realms

ARTFOAV has eight main realms.

### 1. Ashen Veil
Starting realm and surface grave-town.  
City: **Ashvale**  
Safe point: **The Pale Fire**  
Theme: ash roads, looping paths, black-wax rain, cursed survival.

### 2. Maw Narthex
The mouth of the Reliquary.  
Theme: wet stone, tooth-locked doors, hungry architecture.

### 3. Furnace Choir
A burning chapel where punishment became music.  
Theme: fire, smoke hymns, burnt scripture.

### 4. Venom Ossuary
A bone vault soaked in living poison.  
Theme: rot, blood-sickness, corrupted healing.

### 5. Throne of Dead Kings
A royal crypt where every coffin is a soldier.  
Theme: bone armies, dead monarchs, command after death.

### 6. Gilded Gut
A greed realm full of cursed gold and biting chests.  
Theme: treasure goblins, debt demons, realm-local auctions.

### 7. Titan Kiln
A forge-prison built from Hell-Titan remains.  
Theme: giant bones, titan marrow, infernal forging.

### 8. Black Reliquary Heart
The final organ realm.  
Theme: relic teeth, pumping blood chains, Bergus, First Relic, Unborn Below.

## Bosses
Current core bosses:
The Maw Bell Warden
Graxxul, The Jaw Saint
Ignivar, The Cathedral Flame
Venomweeps, The Green Saint
Osseon, The Bone King
Glimmergut, The Debt-Eater
Vorrag, The Split Titan
Bergus The Satanic Arsonist
The Relic-Worn Self
The Unborn Below

World boss concepts after Break the Egg:
Harrowmaw, The Bell-Eater
Saint Ghorvex of Teeth
Pyranth, The Choir Furnace
Mother Veyss, Rot-Womb Saint
King Ossuary Prime
Gildraxx The Coin-Swallower
Kollogar, The Waking Rib
The Thousandth Scream

## Classes and Character Creation
Character creation includes:
name
origin
combat style
appearance

Origin tiers:
6 Royal origins
3 Middle origins
2 Lowly origins

Combat styles:
Brute
Veil Hunter
Gravecaller
Crusader

Appearance:
face/scar
body
arms
legs
hair

Followers and pets are locked until level 10.

## Combat and Progression
Combat is turn-based through the terminal.

Current caps:
Max Level: 100
Max Octa-Level: 1000
XP Cap: 100,000,000
Health Cap: 50,000,000
Mana Cap: 150,000,000

Octa-levels unlock after defeating **Bergus The Satanic Arsonist**.

V5+ tuning lowered early-game damage and mana scaling so levels 1–30 feel more grounded.

Progression feel:
Level 1–10: weak survivor
Level 11–30: competent adventurer
Level 31–50: realm-tested fighter
Level 51–70: boss hunter
Level 71–100: endgame-ready hero
Octa 1–1000: post-Bergus relic exposure progression

## Skills and Skill Tree Direction
Active skills include Brute, Veil Hunter, Gravecaller, Crusader, and realm-themed skills.

Examples:
Crush
Rage Hew
Titan Break
Piercing Shot
Knife Rain
Shadow Step
Bone Swarm
Grave Bolt
Rib Cage
Shield Verdict
Saint Guard
Hammer Psalm
Cinder Slash
Hellbrand Cut
Venom Cut
Goldstorm
Titan Grip
Relic Shriek
Heartchain
Egg Crack

Passive skills include:
Grave Patience
Cinder Hunger
Titan Memory
Royal Debt
Miser's Instinct
Vampiric Habit
Thorned Nerves
Ashen Hide
Black Wax Skin
Saint-Bone Poise
Hellmaw Luck
Coinrot Haggle
Follower Bond
Pet Sense
Stronghold Grit
Boss Hatred
Relic Whisperer
Poison Lessons
Fire Lessons
Bone Lessons

Planned tree paths:
Brute Tree
Veil Hunter Tree
Gravecaller Tree
Crusader Tree
Relic Corruption Tree
Survival Tree
Trade / Utility Tree

## Loot, Gear, Rarities, and Affixes
The loot system generates weapons and armor with rarity, stats, level requirements, power, and affixes.

Rarity stars:
*       Common
**      Uncommon
***     Magic
****    Rare
*****   Epic
******  Legendary
******* Relic

Relic drop chance:
0.0001%

Core affixes:
of Flames
of Vampirism
of Thorns
of the Titan
of Black Wax
of Grave Smoke
of Coinrot

Affix examples:
- **of Flames**: fire damage / burn identity
- **of Vampirism**: lifesteal and blood debt
- **of Thorns**: reflected pain
- **of the Titan**: HP, strength, giant memory
- **of Coinrot**: greed, gold, corruption risk

Weapon types include:
Short Blade
Long Sword
Greatsword
Curved Blade
War Axe
Greataxe
Mace
Maul
Spear
Pike
Scythe
Flail
Crossbow
Longbow
Ritual Knife
Chain Blade
Titan Hammer
Bone Saw
Shield-Blade
Hellbrand Staff

Armor types include:
Cloth Wraps
Leather Vest
Chain Mail
Plate Harness
Cathedral Armor
Bone Cuirass
Hellforged Plate
Venom Ward Robes
Gilded Thief Coat
Royal Crypt Armor
Titan Rib Armor
Reliquary Shell
Demonhide Mantle

## Pets and Followers
Followers unlock at level 10+.

Followers can:
attack
retrieve loot
equip weapons
equip armor
carry lore identity

Pets can:
attack lightly
detect hidden rooms
retrieve small loot
equip armor only
react to cursed relics

Current follower examples:
Mourn Cass
Sella Blackpsalm
Gromm Marrowgate
Velra Coinrot
Odran Saintless
Vaelora Ashmantle

Current pet examples:
Cinder Pup
Mire Moth
Bone Finch
Gutter Gremlin
Ash Lynx
Marrow Whelp

## NPCs, Quests, Economy, and Auction Houses
The world includes generated NPCs per save seed and realm-local NPC pools.

NPC roles include:
blacksmith
potion seller
banker
merchant
auctioneer
relic appraiser
lore keeper
pet handler
follower trainer
bounty master
curse doctor
stronghold scout

Town hub services:
Dungeon gate
Open world map
Player sheet
Inventory
Stash / Iron Maw
Blacksmith
Potion shop
Local auction house
Local NPCs
Quest board
Followers / Pets
Lore codex
Settings
Rest
Save

Auction rule:
Auction houses are realm-local.
Only current realm listings appear.

Main quest structure has 15 chapters:
1. The Roads Curl Back
2. The Door With Teeth
3. The Saint-Bone Nails
4. Hymns Made Of Smoke
5. The Furnace Learns Your Name
6. Blood-Sickness
7. The Green Baptism
8. A Crown For Every Corpse
9. Kneel Before Dust
10. Gold That Bites
11. Glimmergut's Screaming Sack
12. The Rib Forge
13. When Titans Remember How To Stand
14. The Heart Beneath The Heart
15. The Egg Cracks

## Open World Realm Walking
V5.3 adds the first open-world concept.

From the town hub:
Open world map / Walk realms

The player can walk between all eight realms using direction commands.

Current design:
Ashen Veil --- Maw Narthex --- Furnace Choir
     |              |              |
Gilded Gut --- Throne of Dead Kings --- Venom Ossuary
     |              |
Titan Kiln --- Black Reliquary Heart

This shifts ARTFOAV from strict dungeon-only flow toward an **8-bit terminal MMO-style realm map**.

High-danger realms can be reached early, but the road may trigger ambushes.

## Local Server and Login
The local server is real TCP localhost packet exchange.

Default server:
127.0.0.1:47777

Server files:
server_worlds/default/world_state.json
server_worlds/default/accounts/accounts.json
server_worlds/default/sessions/sessions.json

Auth model:
REGISTER
LOGIN
LOGOUT
session token
PBKDF2-SHA256 salted password hashes

Packets:
PING
REGISTER
LOGIN
LOGOUT
HELLO
PLAYER_STATE
CHAT
GET_WORLD
SET_REALM
SERVER_STATUS

Authenticated packets require a token:
HELLO
PLAYER_STATE
CHAT
GET_WORLD
SET_REALM

This is suitable for localhost/LAN testing. It is not internet-grade security yet because there is no TLS, rate limiting, moderation, remote account recovery, or production hosting layer.

## Save / Load

Folders:
saves/
legacy_saves/
imported_original_saves/
server_worlds/

Save data includes:
character stats
appearance
origin
combat style
level
octa-level
XP
gold
gear
inventory
stash
potions
follower
pet
chapter
realm
map state
corruption
boss kills

Old save import:
legacy_saves/ -> imported_original_saves/ -> new V5 save

V5.3 adds save validation:
Validate saves

Validation output:
runtime/save_validation.json

## Audio and Asset System

Audio engine:
artfoav_audio_engine.py

Theme file priority:
custom/sounds/ARTFOAV Soundtrack theme.mp4
assets/sounds/ARTFOAV Soundtrack theme.mp4


Default music volume:
0.04

Audio event queue:
runtime/audio_events_queue.jsonl

Audio event map:
config/audio_events.json

Asset scanner:
artfoav_asset_indexer.py

Asset manifest:
runtime/asset_manifest.json

Audio categories:
theme
menu
attack
hit
victory
pickup
error
run
fall
other

## Settings

Settings file:
config/settings.json

Settings include:

music enabled
SFX enabled
music volume
SFX volume
text speed
CRT mode
autosave
server enabled
server host
server port
session name
open world enabled

The title menu includes a settings menu.

## Folder Structure

Abyssal Reliquary: The Fall of Ashen Veil Official RPG Terminal MMO/
├── artfoav_terminal_v5_bridge.py
├── artfoav_sidecar_ui.py
├── artfoav_launcher.py
├── artfoav_audio_engine.py
├── artfoav_asset_indexer.py
├── artfoav_local_server.py
├── artfoav_server_client.py
├── BUILD_EXE_WINDOWS.bat
├── RUN_GAME_WITH_UI.bat
├── RUN_GAME_WITH_UI_AND_SERVER.bat
├── RUN_LOCAL_SERVER.bat
├── RUN_SERVER_CLIENT.bat
├── RUN_AUDIO_ONLY.bat
├── RUN_ASSET_INDEXER.bat
├── requirements.txt
├── README.md
├── README_FIRST_RUN.txt
├── README_TESTERS.txt
├── KNOWN_ISSUES.txt
├── ARTFOAV_BUG_REPORT_TEMPLATE.txt
├── RELEASE_MANIFEST.json
├── .gitignore
├── config/
├── runtime/
├── saves/
├── legacy_saves/
├── imported_original_saves/
├── server_worlds/
├── server_logs/
├── assets/
├── custom/
├── docs/
└── tester_reports/


## Tester Notes
Use:
RUN_GAME_WITH_UI_AND_SERVER.bat

Test:
first launch
server startup
register/login
character creation
save/load
settings menu
audio engine
Qt UI live-state connection
open world walking
dungeon entry
combat
loot
quest tracker
save validation
EXE build

Bug template:
ARTFOAV_BUG_REPORT_TEMPLATE.txt


## Known Limits

This release candidate is not claiming final MMO completeness.

Known limits:
full synchronized co-op combat is not implemented yet
public internet hosting security is not implemented yet
Qt UI is observer-only
open world walking is first playable concept
some audio events need matching sound files
balancing is not final
skill tree UI is not final
crafting is still basic

## Development Roadmap

Near-term:
1. Stabilize V5.3 tester release
2. Confirm EXE build works on Windows
3. Fix tester-reported launch/audio/server bugs
4. Improve Qt UI styling
5. Expand open world realm map
6. Add quest tracker persistence
7. Add crafting materials inventory
8. Add skill tree screen
9. Add world boss board
10. Begin real party sync layer

Long-term:
full LAN party state
co-op combat rooms
shared auction server
party chat
trade requests
server world events
realm reputation
follower loyalty
pet bonding
New Game+
Octa endgame board

## Honest Release Statement
This is a playable Python-based release candidate with real source files, real server packet exchange, real save/load, real UI bridge files, real audio engine code, and real build scripts.
It is not a finished commercial MMO.
It is a working foundation for testers and future development.
