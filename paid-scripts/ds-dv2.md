---
icon: car-side
---

# DS-DV2

## DS-DV2 Script Configuration

This guide explains all configuration options for the vehicle script using **ESX**. You can adjust settings for debugging, vehicle behavior, commands, notifications, and localization.

***

### 1. Getting Started

The script uses the **ESX framework**. Make sure `es_extended` is installed and running:

```lua
local ESX = exports["es_extended"]:getSharedObject()
Config = {}
```

All configurable options are stored in the `Config` table.

***

### 2. Debug Settings

Enable debug messages for development and troubleshooting.

```lua
Config.Debug = true
```

* **true**: Enables debug messages.
* **false**: Disables debug messages.

***

### 3. Vehicle Settings

These settings control vehicle behavior during respawn, repair, and fuel operations.

| Option              | Description                                                 | Default |
| ------------------- | ----------------------------------------------------------- | ------- |
| `SearchRadius`      | Maximum distance to search for nearby vehicles (in meters). | `10.0`  |
| `DelayAfterDelete`  | Delay in milliseconds after deleting a vehicle.             | `1200`  |
| `DelayBeforeFix`    | Delay before starting vehicle repairs.                      | `500`   |
| `DelayAfterFix`     | Delay after repairs complete.                               | `300`   |
| `DelayBeforeEnter`  | Delay before player can enter the vehicle.                  | `600`   |
| `FixWindows`        | Whether vehicle windows should be repaired.                 | `true`  |
| `WindowCount`       | Number of windows on the vehicle.                           | `4`     |
| `DelayWindow`       | Delay per window repair (ms).                               | `200`   |
| `FixTyres`          | Whether tyres should be repaired.                           | `false` |
| `TyreCount`         | Number of tyres to repair.                                  | `4`     |
| `DelayTyre`         | Delay per tyre repair (ms).                                 | `200`   |
| `RefillFuel`        | Refill vehicle fuel on respawn.                             | `true`  |
| `FuelLevel`         | Fuel level after refill (percentage).                       | `100.0` |
| `DelayFuel`         | Delay for refueling operation.                              | `400`   |
| `RestoreProperties` | Restore custom properties like color and mods.              | `true`  |
| `DelayProps`        | Delay for restoring properties (ms).                        | `350`   |
| `CheckGhostVehicle` | Detect “ghost” vehicles and remove them.                    | `true`  |
| `NetworkMigrate`    | Migrate vehicle network ownership to the player.            | `true`  |

***

### 4. Commands

Commands can be configured for different user groups.

```lua
Config.Commands = {
    RespawnVehicle = {
        Command = "dv2",
        Description = "Respawns the current or closest vehicle and restores it fully.",
        AllowedGroups = { "admin", "superadmin" }
    }
}
```

* **Command**: The text players type to trigger the command.
* **Description**: Explanation of what the command does.
* **AllowedGroups**: Groups allowed to use the command.

***

### 5. Notification Function

Customize how messages are displayed to players.

```lua
Config.Notify = function(msg)
    ESX.ShowNotification(msg)
end
```

You can replace `ESX.ShowNotification` with any other notification system if desired.

***

### 6. Locales (Messages)

All in game messages can be customized here:

```lua
Config.Locale = {
    NoVehicle      = "No vehicle nearby found.",
    GhostDetected  = "Ghost vehicle detected! Removing...",
    VehicleRespawn = "Vehicle is being respawned...",
    DeleteFailed   = "Failed to delete vehicle!",
    SpawnFailed    = "Failed to spawn vehicle!",
    RespawnSuccess = "Vehicle respawned successfully!",
    NoPermission   = "You do not have permission."
}
```

* Customize messages to suit your server’s language and tone.
* Each key corresponds to a specific action or notification.
