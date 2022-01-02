# Teohook Documentation
* [Structs](#structs)
* [Functions](#functions)
* [Hooks](#hooks)

## changelogs

teohook 1.1:
- packet bots
- account creator
- added some stuff to world page
- added few cheats (nazi smoke, anti rapier)
- added error markers to lua

teohook 1.2:
- getItemInfo(int id), worldToScreen(vec2 pos) and isSolid(int x, int y) lua functions
- lua executor for bots
- minimap to world page (click somewhere in minimap to pathfind)
- item database list
- drawing functions (drawline, drawrect, drawtext)
- some imgui lua funcs

teohook 1.3:
Updated version to 3.67
Probably will be the last update :sadge:

## Structs
* [Vector2](#vector2)
* [Vector3](#vector3)
* [NetAvatar](#netavatar)
* [WorldObject](#worldobject)
* [InventoryItem](#inventoryitem)
* [Tile](#tile)
* [GamePacket](#gamepacket)
* [VariantList](#variantlist)
* [ItemInfo](#iteminfo)

## Vector2
| Type | Name | Description|
|:-----|:----:|:-----------|
| Number | `x` | Position x |
| Number | `y` | Position y |

## Vector3
| Type | Name | Description|
|:-----|:----:|:-----------|
| Number | `x` | Position x |
| Number | `y` | Position y |
| Number | `z` | Position z |

## NetAvatar
| Type | Name | Description|
|:-----|:----:|:-----------|
| String | `name` | Player's name |
| String | `country` | Player's flag id |
| [Vector2](#vector2) | `pos`  | Player's position |
| [Vector2](#vector2) | `tile` | Player's tile position |
| [Vector2](#vector2) | `size` | Player's size |
| Number | `netid` | Player's netID |
| Number | `userid` | Player's userID |
| Bool | `facing_left` | Is player facing left |

## WorldObject
| Type | Name | Description|
|:-----|:----:|:-----------|
| Number | `id` | Object's item ID |
| Number | `oid` | Object's index |
| [Vector2](#vector2) | `pos` | Object's position |
| Number | `count` | Object's item count |
| Number | `flags` | Object's flags |
 
## InventoryItem
| Type | Name | Description|
|:-----|:----:|:-----------|
| Number | `id` | Item's ID |
| Number | `count` | Item count |

## Tile
| Type | Name | Description|
|:-----|:----:|:-----------|
| Number | `fg` | Foreground block's ID |
| Number | `bg` | Background block's ID |
| [Vector2](#vector2) | `pos` |Tile's position |
| Number | `flags` | Tile's flags |

## GamePacket
| Type | Name | Description|
|:-----|:----:|:-----------|
| Number | `type` | Packet type |
| Number | ` objtype` |  |
| Number | `count1 ` |  |
| Number | `count2 ` |  |
| Number | `netid ` | Packet netID |
| Number | `item ` |  |
| Number | `flags ` | Packet flags |
| Number | `float1` |  |
| Number | `int_data` |  |
| Number | `pos_x` |  |
| Number | `pos_y` |  |
| Number | `pos2_x` |  |
| Number | `pos2_y` |  |
| Number | `float2` |  |
| Number | `int_x` |  |
| Number | `int_y` |  |

## VariantList
| Type | Name | Description|
|:-----|:----:|:-----------|
| Number | `netid` | NetID |
| Number | `delay` | Delay |
| String | `[0]` | Variant function name |
| Any | `[1]` | Param 1 |
| Any | `[2]` | Param 2 |
| Any | `[3]` | Param 3 |
| Any | `[4]` | Param 4 |
| Any | `[5]` | Param 5 |

## ItemInfo
| Type | Name | Description|
|:-----|:----:|:-----------|
| String | `name` | Item name |
| Number | `id` | Item ID |
| Number | `rarity` | Item rarity |
| Number | `growtime` | Item growtime |
| Number | `breakhits` | Item breakhits |

## Functions
* [sendPacket](#sendpacket)
* [sendPacketRaw](#sendpacketraw)
* [sendVarlist](#sendvarlist)
* [log](#log)
* [findPath](#findpath)
* [getLocal](#getlocal)
* [getInventory](#getinventory)
* [getPlayers](#getplayers)
* [getObjects](#getobjects)
* [getTile](#gettile)
* [getTiles](#gettiles)
* [getItemInfo](#getiteminfo)
* [isSolid](#issolid)
* [drawLine](#drawline)
* [drawText](#drawtext)
* [drawRect](#drawrect)
* [worldToScreen](#worldtoscreen)
* [patchBytes](#patchbytes)


## sendPacket
`sendPacket(bool client, string packet, int type)`

Sends text packet with selected type to client or server, if client is set to true then it sends to client and if its false it sends to server.

Example:
```lua
-- Sends respawn packet to server
sendPacket(false, "action|respawn", 2)
```

## sendPacketRaw
`sendPacketRaw(bool client, GamePacket packet)`

Sends [GamePacket](Structs.md#gamepacket) to server or client, if client is set to true then it sends to client and if its false it sends to server.

Example:
```lua
-- Sends wear clothing packet to server
packet = {}
packet.type = 10 
packet.int_data = 48 -- Clothing ID (Jeans)
sendPacketRaw(false, packet)
```
## sendVarlist
`sendVarlist(Variantlist varlist)`

Sends [Variantlist](Structs.md#variantlist) to client

Example:
```lua
-- Sends OnConsoleMessage to client
varlist = {}
varlist[0] = "OnConsoleMessage" -- Function name
varlist[1] = "Hello!" -- Param 1
sendVarlist(varlist)
```

## log
`log(string message)`

Logs message to Growtopias console (only client side)

Example:
```lua
-- Logs "Hello!" to Growtopias console
log("Hello!")
```

## findPath
`findPath(int x, int y)`

Finds path to selected x,y

Example:
```lua
-- Finds path to top left corner of the world
findPath(0, 0)
```

## getLocal
`getLocal()`

Returns local [NetAvatar](#Structs.md#netavatar) struct

Example:
```lua
-- Logs local players name
me = getLocal()
log(me.name)
```

## getInventory
`getInventory()`

Returns table of [InventoryItems](Structs.md#inventoryitem)

Example:
```lua
-- Logs all item ids in your inventory
for _,cur in pairs(getInventory()) do
log(cur.id)
end
```

## getPlayers
`getPlayers()`

Returns table of [NetAvatars](Structs.md#netavatar)

Example:
```lua
-- Logs current worlds players names
for _,player in pairs(getPlayers()) do
log(player.name)
end
```

## getObjects
`getObjects()`

Returns table of [WorldObjects](Structs.md#worldobject)

Example:
```lua
-- Logs current worlds objects item id's
for _,object in pairs(getObjects()) do
log(object.id)
end
```

## getTile
`getTile(int x, int y)`

Returns world [Tile](Structs.md#tile) in selected position

Example:
```lua
-- Logs top left corners foreground block id
tile = getTile(0, 0)
log(tile.fg)
```

## getTiles
`getTiles()`

Returns table of [Tiles](Structs.md#tile)

Example:
```lua
-- Logs current worlds all foreground block id's
for _,tile in pairs(getTiles()) do
log(tile.fg)
end
```

## getItemInfo
`getItemInfo(int itemid)`

Returns [ItemInfo](Structs.md#iteminfo) of selected Item ID

Example:
```lua
-- Logs Item ID 2's name (Dirt)
log(getItemInfo(2).name)
```

## isSolid
`isSolid(int x, int y)`

Returns true if block is solid and false if its not

Example:
```lua
if isSolid(0, 0) then
log("Tile 0, 0 is solid")
else
log("Tile 0, 0 is not solid")
end
```

## drawLine
`drawLine(int startx, int starty, int endx, int endy, vec3 color)`

Draws line from start pos to end pos (Only can be used in render hook)

Example:
```lua
-- Draws red line from 0,0 to 100,100 screen position
color = {}
color.x = 1
color.y = 0
color.z = 0

function rend()
drawLine(0, 0, 100, 100, color)
end

addHook("Render", "example", rend)
```

## drawText
`drawText(int posx, int posy, string text, vec3 color)`

Draws selected text to selected position (Only can be used in render hook)

Example:
```lua
-- Draws red "Hello" text to 100,100 screen position
color = {}
color.x = 1
color.y = 0
color.z = 0

function rend()
drawText(100, 100, "Hello", color)
end

addHook("Render", "example", rend)
```

## drawRect
`drawRect(int startx, int starty, int endx, int endy, vec3 color)`

Draws rectangle from start pos to end pos (Only can be used in render hook)

Example:
```lua
-- Draws red rectangle from 50,50 to 100,100 screen position
color = {}
color.x = 1
color.y = 0
color.z = 0

function rend()
drawRect(50, 50, 100, 100, color)
end

addHook("Render", "example", rend)
```

## worldToScreen
`worldToScreen(vec2 pos)`

Returns screen coordinates of selected position

Example:
```lua
-- Logs local player x screen position
w2s = worldToScreen(getLocal().pos)
log(w2s.x)
```

## patchBytes
`patchBytes(int address, table bytes)`

Patches selected address's bytes

Example:
```lua
-- Changes memory in address 0x12345 to 0x90, 0x90
bytes = {0x90, 0x90}
patchBytes(0x12345, bytes)
```

## Hooks
* [Usage](#usage)
* [Types](#types)
* [Examples](#examples)

If you return true in hook it wont execute the packet

## Usage
### addHook
`addHook(string type, string name, function)`

Hooks selected function to selected [hook type](#types)

### removeHook
`removeHook(string name)`

Removes hook with selected name

### removeHooks
`removeHooks()`

Removes all hooks

## Types
`Packet`
Arguments: (strint packet, int type)

`OnPacket`
Arguments: (string packet, int type)

`RawPacket`
Arguments: (GamePacket packet)

`OnRawPacket`
Arguments: (GamePacket packet)

`OnVarlist`
Arguments: (VariantList varlist)

`OnTrackPacket`
Arguments: (string packet)

`Render`
Arguments: (none)

## Examples
### Packet
```lua
-- Blocks respawn packet
function hook(packet, type)
if packet:find("action|respawn") then
return true;
end
end

addHook("Packet", "example_hook", hook)
```

### OnPacket
```lua
-- Blocks audios server sends to us
function hook(packet, type)
if packet:find("audio") then
return true;
end
end

addHook("OnPacket", "example_hook", hook)
```

### RawPacket
```lua
-- Blocks packet type 25 
function hook(packet)
if packet.type == 25 then
return true;
end
end

addHook("RawPacket", "example_hook", hook)
```

### OnRawPacket
```lua
-- Blocks packet type 5
function hook(packet)
if packet.type == 5 then
return true
end
end

addHook("OnRawPacket", "example_hook", hook)
```

### OnVarlist
```lua
-- Blocks all dialogs
function hook(varlist)
if varlist[0]:find("OnDialogRequest") then
return true
end
end

addHook("OnVarlist", "example_hook", hook)
```

### OnTrackPacket
```lua
-- Blocks all track packets
function hook(packet)
return true
end

addHook("OnTrackPacket", "example_hook", hook)
```

### Render
```lua
-- Draws red line from 0,0 to 100,100 screen position
color = {}
color.x = 1
color.y = 0
color.z = 0

function hook()
drawLine(0, 0, 100, 100, color)
end

addHook("Render", "example_hook", hook)
```
