# Open Source - Keybinds

Created by LeonMrBonnie

[:heart: Support me by becoming a Patron](https://www.patreon.com/leonmrbonnie/)<br>
⭐ This repository if you found it useful!

[![Generic badge](https://img.shields.io/badge/.altv_Installer%3F-Yes!-4E753E.svg)](https://shields.io/)

---

# Description

This repository provides an alt:V resource to create keybinds.

This resource provides a few easy functions to work with keybinds.

## Installing Dependencies / Installation

**I cannot stress this enough. Ensure you have NodeJS 13+ or you will have problems.**

-   [NodeJS 13+](https://nodejs.org/en/download/current/)
-   An Existing or New Gamemode
-   General Scripting Knowledge


After simply add the name of this resource to your `server.cfg` resource section.

`altv-os-keybinds`

Then simply clone this repository into your main server resources folder.

```
cd resources
git clone https://github.com/LeonMrBonnie/altv-os-keybinds
```

Ensure your `package.json` includes this property:

```json
"type": "module"
```
# How To Use

This resource is fully **clientside**. It has no serverside code.<br>
To use this resource, you have to add it to the `deps` array in the `resource.cfg` of the resource, where you want to use the keybinds.<br>
Then you can import the resource by using `import * as Keybinds from "altv-os-keybinds"`

# Available functions

`registerKeybind`: (Arguments starting with a `?` are optional)

| Argument          | Description                                   | Type        |
| ----------------- | --------------------------------------------- | ----------- |
| `key`             | Key name (E.g. 'E')                           | `String`    |
| `hold`            | Does the key need to be held                  | `Boolean`   |
| `debounce`        | Debounce time for this keybind                | `Number`    |
| `onRelease`       | Executed on key release                       | `Function`  |
| `?onPress`        | (Only for hold = true) Executed on key press  | `Function`  |

Returns: `Keybind id`

Example:
```js
import * as Keybinds from "altv-os-keybinds";

// Register a keybind for the key E
let myKeybind = Keybinds.registerKeybind("E", true, 500, 
() => {
    alt.log(`Released key E`);
}, 
() => {
    alt.log(`Pressed key E`);
});
```

---

`unregisterKeybind`: (Arguments starting with a `?` are optional)

| Argument          | Description                                   | Type        |
| ----------------- | --------------------------------------------- | ----------- |
| `keybindId`       | Keybind id (Returned by `registerKeybind`)    | `Number`    |

Returns: `Nothing`

Example:
```js
import * as Keybinds from "altv-os-keybinds";

let myKeybind = Keybinds.registerKeybind("E", false, 0, () => {
    Keybinds.unregisterKeybind(myKeybind); // Remove the keybind after the first press
});
```

---

`isKeybindHeld`: (Arguments starting with a `?` are optional)

| Argument          | Description                                   | Type        |
| ----------------- | --------------------------------------------- | ----------- |
| `keybindId`       | Keybind id (Returned by `registerKeybind`)    | `Number`    |

Returns: `Boolean indicating if the key is currently held`

*Notice*: This only works for keybinds with `hold = true`.

Example:
```js
import * as alt from "alt-client";
import * as Keybinds from "altv-os-keybinds";

let myKeybind = Keybinds.registerKeybind("E", true, 0, () => {}, () => {});

// Only allow the player to move when the E key is held
alt.everyTick(() => {
    let gameControlsActive = alt.gameControlsEnabled();
    if(Keybinds.isKeybindHeld(myKeybind) && !gameControlsActive) alt.toggleGameControls(true);
    else if(gameControlsActive) alt.toggleGameControls(false);
});
```

---

`getKeybindsFromKey`: (Arguments starting with a `?` are optional)

| Argument          | Description            | Type        |
| ----------------- | ---------------------- | ----------- |
| `key`             | Key name (E.g. 'E')    | `String`    |

Returns: `Array containing all keybind ids with the specified key`

Example:
```js
import * as alt from "alt-client";
import * as Keybinds from "altv-os-keybinds";

Keybinds.registerKeybind("E", true, 0, () => {}, () => {});
Keybinds.registerKeybind("E", true, 0, () => {}, () => {});
Keybinds.registerKeybind("E", true, 0, () => {}, () => {});
Keybinds.registerKeybind("E", true, 0, () => {}, () => {});
Keybinds.registerKeybind("E", true, 0, () => {}, () => {});

let keybinds = Keybinds.getKeyBindsFromKey("E"); // Array containing the 5 keybind ids
```

---

`setKeybindDisabled`: (Arguments starting with a `?` are optional)

| Argument          | Description                                  | Type        |
| ----------------- | -------------------------------------------- | ----------- |
| `keybindId`       | Keybind id (Returned from `registerKeybind`) | `Number`    |
| `disabled`        | Should the keybind be disabled               | `Boolean`   |

Returns: `Nothing`

Example:
```js
import * as alt from "alt-client";
import * as Keybinds from "altv-os-keybinds";

let keybind = Keybinds.registerKeybind("E", true, 0, () => {}, () => {});

Keybinds.setKeybindDisabled(keybind, true); // Disables the keybind
```

---

`isKeybindDisabled`: (Arguments starting with a `?` are optional)

| Argument          | Description                                  | Type        |
| ----------------- | -------------------------------------------- | ----------- |
| `keybindId`       | Keybind id (Returned from `registerKeybind`) | `Number`    |

Returns: `Boolean indicating if the keybind is disabled`

Example:
```js
import * as alt from "alt-client";
import * as Keybinds from "altv-os-keybinds";

let keybind = Keybinds.registerKeybind("E", true, 0, () => {}, () => {});

alt.log(Keybinds.isKeybindDisabled(keybind)); // Logs 'false'
Keybinds.setKeybindDisabled(keybind, true); // Disables the keybind
alt.log(Keybinds.isKeybindDisabled(keybind)); // Logs 'true'
```

---

`setKeyDisabled`: (Arguments starting with a `?` are optional)

| Argument          | Description                                  | Type        |
| ----------------- | -------------------------------------------- | ----------- |
| `key`             | Key name (E.g. 'E')                          | `Number`    |
| `disabled`        | Should the key be disabled                   | `Boolean`   |

Returns: `Nothing`

Example:
```js
import * as alt from "alt-client";
import * as Keybinds from "altv-os-keybinds";

Keybinds.registerKeybind("E", true, 0, () => {}, () => {});
Keybinds.registerKeybind("E", true, 0, () => {}, () => {});
Keybinds.registerKeybind("E", true, 0, () => {}, () => {});
Keybinds.registerKeybind("E", true, 0, () => {}, () => {});
Keybinds.registerKeybind("E", true, 0, () => {}, () => {});

Keybinds.setKeyDisabled("E", true); // Disables all the 5 created keybinds
```

# Other alt:V Open Source Resources

-   [Authentication by Stuyk](https://github.com/Stuyk/altv-os-auth)
-   [Discord Authentication by Stuyk](https://github.com/Stuyk/altv-discord-auth)
-   [Global Blip Manager by Dzeknjak](https://github.com/jovanivanovic/altv-os-global-blip-manager)
-   [Global Marker Manager by Dzeknjak](https://github.com/jovanivanovic/altv-os-global-marker-manager)
-   [Chat by Dzeknjak](https://github.com/jovanivanovic/altv-os-chat)
-   [Nametags by Stuyk](https://github.com/Stuyk/altv-os-nametags)
-   [Entity Sync for JS by LeonMrBonnie](https://github.com/LeonMrBonnie/altv-os-js-entitysync)
-   [Context Menu by Stuyk](https://github.com/Stuyk/altv-os-context-menu)
-   [Global Textlabels by Stuyk](https://github.com/Stuyk/altv-os-global-textlabels)
-   [Interactions by LeonMrBonnie](https://github.com/LeonMrBonnie/altv-os-interactions)
-   [Noclip by LeonMrBonnie](https://github.com/LeonMrBonnie/altv-os-noclip)
-   [Character Editor by Stuyk](https://github.com/Stuyk/altv-os-character-editor)