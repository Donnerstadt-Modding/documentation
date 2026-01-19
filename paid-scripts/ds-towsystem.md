---
description: >-
  This page explains every configuration option for the towing system. Modify
  these values to customize the script.
icon: truck-tow
---

# DS-Towsystem

### Config Debug

#### `Config.Debug`

Enable or disable debug print statements.

***

### Job Lock

#### `Config.AllowedJobs`

```lua
Config.AllowedJobs = {
    mechanic = true,
    tow = true
}
```

Only players with jobs listed here can use the towing system.\
Set job names to `true` to allow access.

***

### Tow Menu Settings

#### `Config.TowMenu.EnableAnimation`

Enable or disable the towing animation.

***

#### `Config.TowMenu.AnimationType`

Choose the animation type. Options:

* `"clipboard"`
* `"tablet"`

***

#### `Config.TowMenu.AnimationTime`

Animation duration in milliseconds.

***

#### `Config.TowMenu.SearchDistance`

Maximum distance to search for nearby vehicles to load onto the flatbed.

***

### Translations

#### `Config.Locale.VEHICLE_LOADED`

Message shown when a vehicle is successfully loaded.

***

#### `Config.Locale.VEHICLE_LOADING`

Message shown while the vehicle is loading.

***

#### `Config.Locale.VEHICLE_ALREADY_LOADED`

Message shown when a vehicle is already loaded on the flatbed.

***

#### `Config.Locale.VEHICLE_NEARBY_NONE`

Message shown when no vehicle is found nearby.

***

#### `Config.Locale.VEHICLE_DROPPED`

Message shown when a vehicle is unloaded.

***

#### `Config.Locale.MENU_TITLE`

Title shown in the tow menu.

***

#### `Config.Locale.MENU_DISTANCE`

Format string for distance display in the menu.

***

### Notification Settings

#### `Config.NotifyConfig.Enabled`

Enable or disable notifications.

***

#### `Config.NotifyConfig.Type`

Choose notification type. Options:

* `'lib'`
* `'esx'`
* `'chat'`
* `'custom'`

***

#### `Config.NotifyConfig.Title`

Title shown in notifications.

***

### Notification Function

#### `Config.Notify(keyOrMsg, nType)`

This function sends notifications based on `Config.NotifyConfig.Type`.

* If `keyOrMsg` matches a key in `Config.Locale`, it uses the translation.
* Otherwise, it uses `keyOrMsg` as a custom message.
* `nType` sets the notification type (`info`, `success`, `error`, etc.).

***

### Target Options

#### `Config.Targets.LOAD_LABEL`

Label shown on the target for loading a vehicle.

***

#### `Config.Targets.LOAD_ICON`

Icon shown on the target for loading a vehicle.

***

#### `Config.Targets.DROP_LABEL`

Label shown on the target for unloading a vehicle.

***

#### `Config.Targets.DROP_ICON`

Icon shown on the target for unloading a vehicle.

***

### Flatbed Configuration

#### `Config.Flatbeds`

Defines vehicles that can be used as flatbeds and their offsets.

Example:

```lua
Config.Flatbeds = {
    [`flatbed`] = {
        vehicleOffset = vec3(0.0, -3.5, 1.0),
        dropOffsetY = 5.0
    },
    [`actschlepp`] = {
        vehicleOffset = vec3(0.0, -3.5, 1.0),
        dropOffsetY = 5.0
    }
}
```

***

#### `vehicleOffset`

Type: `vector3`\
Default: `vec3(0.0, -3.5, 1.0)`

Offset for loading the vehicle onto the flatbed.

***

#### `dropOffsetY`

Offset used when dropping a vehicle from the flatbed.

***

### Animation Props

#### `Config.Props`

Defines animation props for the tow menu.

Example:

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

***

