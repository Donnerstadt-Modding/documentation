---
icon: shield-quartered
---

# DS-SilentHands

## DS-SilentHands

**DS-SilentHands** is a FiveM script that makes players incapacitated, restricts their controls, and displays a countdown timer via the NUI interface.\
All configuration is handled through the `config.lua` file. Each option is explained in detail below.

***

### Requirements

* ESX Legacy
* ox\_inventory
* visn\_are

***

### Configuration (config.lua)

#### 1. Display Language

```lua
Config.Language = 'de'
```

**Description:**\
Sets the language used for the NUI display.

**Available values:**\
`'de'`, `'en'`, or any custom language defined in the `locales` folder.

**Example:**

```lua
Config.Language = 'en' -- Display in English
```

***

#### 2. Incapacitation Duration

```lua
Config.IncapacitatedTime = 600
```

**Description:**\
Defines how long (in seconds) a player remains incapacitated.

**Examples:**

* `600` → 10 minutes
* `300` → 5 minutes

**Note:**\
This value is automatically shown as a countdown timer in the NUI display.

***

#### 3. NUI Display Settings

```lua
Config.NUI = {
    left = 20,
    top = 50,
    width = 200,
    showMinutesSeconds = true
}
```

**3.1 left**

Horizontal position of the status box in pixels.\
Example: `left = 50` → 50 pixels from the left edge.

**3.2 top**

Vertical position of the status box (in percent).\
Example: `top = 50` → Centered vertically.\
Note: `0%` = top of the screen, `100%` = bottom.

**3.3 width**

Width of the status box in pixels.\
Example: `width = 250` → Status box is 250 pixels wide.

**3.4 showMinutesSeconds**

Defines how the timer is displayed.

* `true` → MM:SS (minutes:seconds)
* `false` → Seconds only

**Example:**

```lua
showMinutesSeconds = false
```

***

#### 4. Blocked Controls

```lua
Config.BlockedControls = {
    24,  -- Attack
    25,  -- Aim
    47,  -- Weapon
    140, -- Melee Attack 1
    141, -- Melee Attack 2
    142, -- Melee Attack 3
    143, -- Melee Attack 4
    257, -- Attack 3
    263, -- Melee Attack 5
    264  -- Melee Attack 6
}
```

**Description:**\
List of control IDs that are disabled while the player is incapacitated.

**Common controls:**

* 24 → Attack
* 25 → Aim
* 47 → Weapon
* 140–143, 257, 263, 264 → Melee attacks

**Note:**\
You can add or remove any control IDs as needed.

***

#### 5. Allowed Weapons

```lua
Config.AllowedWeapons = {
    [GetHashKey("WEAPON_KNIFE")] = true
}
```

**Description:**\
Specifies which weapons a player is allowed to keep while incapacitated.

**Example:**

```lua
Config.AllowedWeapons = {
    [GetHashKey("WEAPON_KNIFE")] = true,
    [GetHashKey("WEAPON_PISTOL")] = true
}
```

**Note:**\
Use `GetHashKey("WEAPON_NAME")` for each allowed weapon.

***

#### 6. Admin Command: End Incapacitation

Admins with the `admin` group can manually remove the incapacitated state from a player.

**Command:**

```
/removeincap [playerId]
```

**Description:**\
Ends the incapacitated state for a specific player.

**Example:**

```
/removeincap 12
```

***

### Exports (for external resources)

Other scripts (such as events, paintball systems, etc.) can directly interact with this resource using the provided exports.

***

#### Client Exports

**Check Status**

```lua
exports['ds-silenthands']:IsIncapacitated() -- returns true/false
exports['ds-silenthands']:GetTimer()        -- returns remaining seconds
```

**Force Actions**

```lua
exports['ds-silenthands']:ForceDisable()      -- Force a player into incapacitation
exports['ds-silenthands']:ForceRemoveIncap()  -- Instantly remove incapacitation
```

**UI Handling**

```lua
exports['ds-silenthands']:SetSkipUI(true)   -- Disable UI display
exports['ds-silenthands']:ShouldSkipUI()    -- Returns whether UI is disabled
```

**Reset**

```lua
exports['ds-silenthands']:ResetOverrides()  -- Reset all overrides to default
```

***

#### Server Exports

**Manage Incapacitation**

```lua
exports['ds-silenthands']:RemoveIncapFromPlayer(playerId) -- Remove incapacitation
exports['ds-silenthands']:ForceIncapOnPlayer(playerId)    -- Force incapacitation
```
