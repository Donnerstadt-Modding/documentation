---
icon: toolbox
---

# DS-Repairkit

## DS-RepairKit – Configuration & Guide

This page explains how to configure and use **DS-RepairKit** on your server.

***

### Requirements

* ESX Legacy
* ox\_target
* (Optional) lation\_ui

***

### Configuration Options

All settings are located in the `config.lua` file.

***

#### Job Settings

```lua
Config.AllowedJob = "rr"          -- Job allowed to use the RepairKit
Config.MaxAllowedOnDuty = 1       -- Maximum number of players with this job on duty
```

**Description:**

* `AllowedJob` → Name of the job that is allowed to use the repair kit
* `MaxAllowedOnDuty` → Maximum number of players with that job simultaneously on duty

***

#### Repair Settings

```lua
Config.RepairDuration = 5000      -- Repair duration in milliseconds
Config.RepairItem = "repair_kit"  -- Item required to repair vehicles
```

**Description:**

* `RepairDuration` → How long the repair takes (in milliseconds)
* `RepairItem` → Item name that players need in their inventory to repair a vehicle

***

#### Localization / Texts

```lua
Config.Locales = {
    ["not_allowed"] = "Too many players with the job '%s' are already on duty!",
    ["used_repairkit"] = "You have used a repair kit.",
    ["repairing"] = "Repairing vehicle...",
    ["no_vehicle"] = "No vehicle nearby.",
    ["cancelled"] = "Repair cancelled.",
    ["no_kit"] = "You do not have a repair kit.",
    ["fixed"] = "Vehicle has been repaired.",
    ["fixcar"] = "Repair vehicle"
}
```

**Description:**

* Customize these texts to match your server language.
* All keys can be translated for multi-language support.

***

#### Notifications

```lua
Config.NotifyType = "ox" -- Options: ox, esx, custom

function Notify(msg, type)
    if Config.NotifyType == "ox" then
        lib.notify({description = msg, type = type})
    elseif Config.NotifyType == "esx" then
        ESX.ShowNotification(msg)
    elseif Config.NotifyType == "custom" then
        TriggerEvent('custom_notify', msg, type)
    end
end
```

**Description:**

* `ox` → Uses `lib.notify`
* `esx` → Uses `ESX.ShowNotification`
* `custom` → Allows your own custom notification events

***

### Installation

1. Copy the script into your server resources folder (`resources/DS-Repairkit`)
2.  Add the following to `server.cfg`:

    ```txt
    ensure DS-Repairkit
    ```
3. Adjust `config.lua` according to your server setup
4. Add required items (e.g., `repair_kit`) to your inventory system
5. Start the server and test functionality

***

### Tips & Recommendations

* Make sure the job in `Config.AllowedJob` exists on your server
* Adjust `Config.RepairDuration` to set realistic repair times
* Use `Config.NotifyType` to match your server’s notification system
* Translate `Config.Locales` for other languages if needed
