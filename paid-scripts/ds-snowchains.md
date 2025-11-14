---
icon: snowflake
---

# DS-Snowchains

This page explains how the **Snow Chains Script** works, what each configuration option does, and how to use it in your FiveM server.

***

### Overview

The Snow Chains Script allows specific jobs to install and remove snow chains on vehicles during snowy weather. It integrates with **ESX** and optionally uses a progress bar or notifications.

***

### Config Table

All settings are stored in the `Config` table. Below is a breakdown of each configuration option.

#### General Settings

| Setting              | Type    | Description                                                                         |
| -------------------- | ------- | ----------------------------------------------------------------------------------- |
| `Config.Debug`       | boolean | Enables debug mode. Prints messages to the console for testing and troubleshooting. |
| `Config.DebugNotify` | boolean | Shows debug messages as in-game notifications (can be spammy).                      |
| `Config.CommandName` | string  | The command used to toggle snow chains (e.g., `/snowchain`).                        |

***

#### Allowed Jobs

```lua
Config.AllowedJobs = {
    ["police"]    = true,
    ["ambulance"] = true,
    ["mechanic"]  = true
}
```

* **Description:** Only the listed jobs can use snow chains.
* **Supported jobs:** `police`, `ambulance`, `mechanic`.

***

#### Vehicle Requirement

```lua
Config.RequireInVehicle = true
```

* **Description:** Players must be inside a vehicle to install or remove snow chains.

***

#### Progress Bar Settings

```lua
Config.Progress = {
    useCustom       = false,
    duration        = 5000,
    labelInstall    = "Snow chains are being installed...",
    labelRemove     = "Snow chains are being removed...",
    Custom          = function(duration, label)
        print("Config.Progress.Custom fallback running for " .. tostring(duration) .. "ms label: " .. tostring(label))
        Citizen.Wait(duration)
        return true
    end
}
```

* **`useCustom`**: Set to `true` if you want to use a custom progress function.
* **`duration`**: Time in milliseconds for the install/remove action.
* **`labelInstall`**: Text shown while installing snow chains.
* **`labelRemove`**: Text shown while removing snow chains.
* **`Custom`**: Optional fallback function that runs if no custom progress system is provided. Currently uses `Citizen.Wait()` to simulate progress.

***

#### Notifications

```lua
Config.Notify = function(msg, type)
    if ESX and ESX.ShowNotification then
        ESX.ShowNotification(msg)
    else
        print("NOTIFY:", type or "info", msg)
    end
end
```

* **Description:** Handles notifications. Uses ESX notifications if available, otherwise prints to console.
* **Parameters:**
  * `msg`: The message text.
  * `type`: Optional type (`info`, `error`, etc.) for logging.

***

#### Snow Weather Requirement

```lua
Config.SnowWeatherRequired = true
Config.AllowedSnowWeather = {
    XMAS     = true,
    SNOW     = true,
    BLIZZARD = true
}
```

* **Description:** Snow chains can only be installed in specific weather conditions.
* **Allowed weather types:**
  * `XMAS` – Christmas snow
  * `SNOW` – Regular snow
  * `BLIZZARD` – Blizzard

***

#### Vehicle Handling Settings

```lua
Config.SnowHandling = {
    fTractionCurveMax       = 3.2,
    fTractionCurveMin       = 2.8,
    fTractionBiasFront      = 0.48,
    fLowSpeedTractionLossMult = 0.0
}
```

* **Description:** Adjusts vehicle physics while snow chains are active.
* **Parameters:**
  * `fTractionCurveMax` – Maximum traction value.
  * `fTractionCurveMin` – Minimum traction value.
  * `fTractionBiasFront` – Traction distribution between front and rear wheels.
  * `fLowSpeedTractionLossMult` – Multiplier for low-speed traction loss.

***

#### Localized Messages

```lua
Config.Locale = {
    noVehicle = "You must be inside a vehicle!",
    noJob     = "Your job is not allowed to use snow chains!",
    noSnow    = "There is no snow!",
    installed = "Snow chains installed!",
    removed   = "Snow chains removed!"
}
```

* **Description:** Customizable messages shown to players.
* **Messages:**
  * `noVehicle` – Player tried to install/remove chains outside a vehicle.
  * `noJob` – Player's job is not allowed.
  * `noSnow` – Snow is not present.
  * `installed` – Successfully installed snow chains.
  * `removed` – Successfully removed snow chains.

***

### How It Works

1. Player uses the `/snowchain` command.
2. The script checks if the player is in a vehicle and has an allowed job.
3. The weather is checked .
4. If all checks pass, a progress bar or fallback simulates installing/removing snow chains.
5. Notifications inform the player of success or errors.
6. Vehicle handling values are adjusted while snow chains are active.

***

### Summary

* **Purpose:** Make vehicles safer in snowy weather.
* **Customizable:** Jobs, messages, progress system, handling, weather types.
* **Integration:** Works with ESX and can be extended to other frameworks.
