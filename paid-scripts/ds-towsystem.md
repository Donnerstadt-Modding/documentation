---
icon: truck-tow
---

# DS-Towsystem

## Towing System Configuration

This page documents all configurable options for the Towing System. Modify these settings to customize behavior, animations, notifications, and more.

***

### Debug Mode

```lua
Config.Debug = true
```

Enable or disable debug logging. When `true`, debug messages are printed to help with troubleshooting.

***

### Job Lock

```lua
Config.AllowedJobs = {
    rr = true,
    tow = true
}
```

Restrict towing functionality to specific jobs. Only players with jobs listed here can access the tow menu.

***

### Tow Menu

```lua
Config.TowMenu = {
    EnableAnimation = true,
    AnimationType = "clipboard",  -- Options: "clipboard", "tablet", "none"
    AnimationTime = 3000,         -- Duration of animation in milliseconds
    SearchDistance = 12.0         -- Vehicle detection radius
}
```

* **EnableAnimation:** Enable or disable animation when interacting with the tow menu.
* **AnimationType:** Choose `"clipboard"`, `"tablet"`, or `"none"`.
* **AnimationTime** : How long the animation plays in milliseconds.
* **SearchDistance**: Radius around the player to detect vehicles for towing.

***

### Notifications

#### Locale Messages

```lua
Config.Locale = {
    VEHICLE_LOADED          = "Vehicle loaded!",
    VEHICLE_LOADING         = "Loading vehicle...",
    VEHICLE_ALREADY_LOADED  = "A vehicle is already loaded!",
    VEHICLE_NEARBY_NONE     = "No vehicle nearby!",
    VEHICLE_DROPPED         = "Vehicle dropped!"
}
```

Customize the notification messages displayed during towing actions.

#### Notification Type

```lua
Config.NotifyType = 'lib' -- Options: 'lib', 'esx', 'chat'
```

* **lib** = Uses `lib.notify` for advanced notifications.
* **esx** = Uses ESX notifications.
* **chat** = Sends messages to chat.
* **Custom** = Integrate your own

#### Notify Function

```lua
local function notify(msg, type)
    cdebug("NOTIFY ("..(type or "info").."): " .. msg)
    if Config.NotifyType == 'lib' and lib then
        lib.notify({title='Towing', description=msg, type=type or 'info'})
    elseif Config.NotifyType == 'esx' then
        ESX.ShowNotification(msg)
    else
        TriggerEvent('chat:addMessage', {args={msg}})
    end
end
```

This function sends notifications based on the selected system.

***

### Targets

```lua
Config.Targets = {
    LOAD_LABEL = "Load Vehicle",
    LOAD_ICON  = "fa-solid fa-truck",
    DROP_LABEL = "Drop Vehicle",
    DROP_ICON  = "fa-solid fa-truck-ramp-box"
}
```

* **LOAD\_LABEL / DROP\_LABEL** = Text shown on interaction targets.
* **LOAD\_ICON / DROP\_ICON** = Icon displayed for the corresponding action.

***

### Flatbeds

```lua
Config.Flatbeds = {
    [`actschlepp`] = {
        rampBone = 'misc_z',          -- Ramp bone name (Not implementet)
        rampUpRot = 0.0,               -- Ramp rotation when up (Not implementet)
        rampDownRot = -25.0,           -- Ramp rotation when down (Not implementet)
        vehicleOffset = vec3(0.0, -3.5, 1.0), -- Vehicle placement on flatbed
        dropOffsetY = 5.0              -- Distance behind flatbed when dropping vehicle
    }
}
```

Defines flatbed-specific settings for vehicle towing.

***

### Animation Props

```lua
Config.Props = {
    clipboard = {
        model = `p_cs_clipboard`,
        animDict = "amb@world_human_clipboard@male@base",
        animName = "base",
        bone = 60309,
        offset = vector3(0.08, 0.02, 0.0),
        rotation = vector3(20.0, 0.0, 0.0)
    },
    tablet = {
        model = `prop_cs_tablet`,
        animDict = "amb@code_human_in_bus_passenger_idles@female@tablet@base",
        animName = "base",
        bone = 60309,
        offset = vector3(0.08, 0.0, -0.03),
        rotation = vector3(180.0, 0.0, 0.0)
    }
}
```

* **model** = The prop model to attach.
* **animDict / animName** = Animation dictionary and name for the interaction.
* **bone** = The bone on the player to attach the prop.
* **offset / rotation** = Position and rotation relative to the bone.
