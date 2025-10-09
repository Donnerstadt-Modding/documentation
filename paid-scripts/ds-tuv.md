---
icon: car
---

# DS-TÜV

## DS-TÜV Script

***

### Requirements

* ESX Legacy
* ox\_lib
* oxmysql
* ox\_target
* lation\_ui (Optional)

***

### SQL Setup (REQUIRED)

Before starting, make sure to create the following table in your database:

```sql
CREATE TABLE IF NOT EXISTS `tuv_registrations` (
  `id` INT NOT NULL AUTO_INCREMENT,
  `name` VARCHAR(100) NOT NULL,
  `plate` VARCHAR(20) NOT NULL,
  `duration_days` INT NOT NULL,
  `expiry` BIGINT NOT NULL,
  `answers` TEXT,
  `created_at` BIGINT NOT NULL,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4;
```

This table is used to store all TÜV registrations, including player names, license plates, validity duration, and timestamps.

***

### 1. General Settings

```lua
Config.DefaultLanguage = 'de'   -- Default language
Config.Debug = true             -- Enable debug mode (true/false)
Config.UIFramework = 'ox_lib'   -- Options: 'ox_lib' or 'lation_ui'
```

* **DefaultLanguage**: Sets the default language for the script.
* **Debug**: Enables console messages and debug information.
* **UIFramework**: Specifies the UI framework to use.

***

### 2. Database

```lua
Config.Database = {
    TableName = "tuv_registrations"
}
Config.TableName = Config.Database.TableName
```

* **TableName**: Name of the database table for TÜV registrations.

***

### 3. Jobs

```lua
Config.Jobs = {
    TUVRegister = "rr",      -- Job allowed to register vehicles
    TUVCheck    = "police"   -- Job allowed to check vehicles
}
```

* **TUVRegister**: Job that can register new TÜVs.
* **TUVCheck**: Job that can inspect vehicles for TÜV.

***

### 4. TÜV Durations

```lua
Config.Durations = {
    Allowed = {30, 60, 90, 120, 180}, -- Allowed durations in days
    MaxMonths = 5                     -- Maximum duration in months
}
```

* **Allowed**: List of allowed TÜV durations in days.
* **MaxMonths**: Maximum duration allowed in months.

***

### 5. Location

```lua
Config.Location = {
    Position = vector3(-333.1791, -133.1168, 39.0097),
    Target = {
        Radius  = 4.0,
        Height  = 1.5,
        Debug   = false,
        Options = {
            Icon = 'fa-solid fa-car'
        }
    }
}
```

* **Position**: Coordinates of the TÜV location.
* **Target**: Settings for the interaction zone.
  * `Radius` → Interaction radius
  * `Height` → Height of the zone
  * `Debug` → Visibility for debugging
  * `Options.Icon` → Icon for the target point

***

### 6. Self-TÜV

```lua
Config.SelfTUV = {
    Enabled = true,
    Mode = "target_only", -- Options: 'ped_target', 'marker', 'target_only'
    Ped = { ... },
    Marker = { ... },
    Target = { ... },
    Settings = { ... },
    LationUIMenu = { ... }
}
```

* **Enabled**: Activates the self-TÜV feature.
* **Mode**: Type of interaction:
  * `ped_target` → Ped + Target
  * `marker` → Marker + Press E
  * `target_only` → Target only

#### 6.1 Ped Settings

```lua
Ped = {
    Model = "s_m_m_autoshop_01",
    Position = vector3(-345.12, -133.56, 38.0),
    Heading = 90.0
}
```

* Ped model and position for self-TÜV interaction.

#### 6.2 Marker Settings

```lua
Marker = {
    Type = 1,
    Scale = vector3(1.0, 1.0, 1.0),
    Color = {r = 0, g = 255, b = 0, a = 100},
    Position = vector3(-345.12, -133.56, 38.0)
}
```

* Marker type, color, scale, and position.

#### 6.3 Target Settings

```lua
Target = {
    Icon = 'fa-solid fa-wrench',
    IconColor = '#4CAF50'
}
```

* Icon and color for the interaction target.

#### 6.4 Interaction Settings

```lua
Settings = {
    InteractionRadius = 10.0,
    NotifyPosition = 'top-center',
    Scenario = 'WORLD_HUMAN_CLIPBOARD'
}
```

* Interaction radius, notification position, and ped scenario.

#### 6.5 Lation UI Menu

```lua
LationUIMenu = {
    ID = "self_tuv_menu",
    Title = "Self TÜV",
    Subtitle = "Select the duration of your TÜV",
    ColorPrimary = "#4CAF50",
    Options = {
        { title = "5 Days", duration = 5, icon = "fas fa-calendar-check", enabled = true },
        { title = "10 Days", duration = 10, icon = "fas fa-calendar-days", enabled = true },
    }
}
```

* Menu ID, title, primary color, and menu options.

***

### 7. TÜV Questions

```lua
Config.Questions = {
    {id = "q_brakes",     required = true},
    {id = "q_lights",     required = true},
    {id = "q_tires",      required = true},
    {id = "q_emissions",  required = false},
    {id = "q_oil",        required = false},
    {id = "q_wipers",     required = false},
    {id = "q_wheels",     required = false},
    {id = "q_horn",       required = false},
    {id = "q_windows",    required = false},
    {id = "q_suspension", required = false},
}
```

* `id` → Internal question ID
* `required` → Is the question mandatory (true/false)

***

### 8. Server Messages

```lua
Config.ServerMessages = {
    db_error_insert = '❌ Database error while inserting',
    db_error_delete = '❌ Database error while deleting'
}
```

* Messages for database errors.

***

### 9. Server Messages

The TÜV script provides several exported functions for use by other resources or scripts.

***

### 9.1 HasRegisterJob

```lua
exports('HasRegisterJob', HasRegisterJob)
```

* **Purpose:** Checks if a player has the job required to register vehicles.
* **Returns:** Boolean (`true` / `false`)

***

### 9.2 HasCheckJob

```lua
exports('HasCheckJob', HasCheckJob)
```

* **Purpose:** Checks if a player has the job required to inspect vehicles.
* **Returns:** Boolean (`true` / `false`)

***

### 9.3 OpenSelfTUVMenu

```lua
exports('OpenSelfTUVMenu', function(playerId) 
    TriggerClientEvent('tuv:openSelfMenu', playerId) 
end)
```

* **Purpose:** Opens the self-TÜV menu for a specific player.
* **Parameters:** `playerId` (server ID of the player)

***

### 9.4 SubmitTUV

```lua
exports('SubmitTUV', InsertOrUpdateTUV)
```

* **Purpose:** Registers or updates a vehicle's TÜV in the database.
* **Parameters:** Data table containing vehicle information

***

### 9.5 UnregisterTUV

```lua
exports('UnregisterTUV', function(playerId, plate) 
    TriggerEvent('tuv:abmelden', {plate = plate}) 
end)
```

* **Purpose:** Removes or unregisters a vehicle from TÜV.
* **Parameters:** `playerId` (server ID), `plate` (vehicle plate)

***

### 9.6 GetTUVStatus

```lua
exports('GetTUVStatus', function(plate, cb) 
    ESX.TriggerServerCallback('tuv:getStatus', cb, plate) 
end)
```

* **Purpose:** Retrieves the TÜV status of a vehicle.
* **Parameters:** `plate` (vehicle plate), `cb` (callback function)

***

### 9.7 IsOwnedVehicle

```lua
exports('IsOwnedVehicle', function(playerId, plate, cb) 
    ESX.TriggerServerCallback('tuv:isOwnedVehicle', cb, plate) 
end)
```

* **Purpose:** Checks if a vehicle is owned by a player.
* **Parameters:** `playerId` (server ID), `plate` (vehicle plate), `cb` (callback function)

***

### 9.8 FormatPlate

```lua
exports('FormatPlate', FormatPlate)
```

* **Purpose:** Returns a formatted version of a license plate.
* **Parameters:** Plate string
* **Returns:** Formatted plate string

***

### 9.9 Debug

```lua
exports('Debug', AdvDebugLog)
```

* **Purpose:** Outputs advanced debug information to the console.
* **Parameters:** Message or data to log

***

### 10. Tips & Recommendations

* **Enable Debug Mode:**\
  Set `Config.Debug = true` to view helpful console logs for troubleshooting.
* **Verify Job Names:**\
  Ensure that `TUVRegister` and `TUVCheck` exist and match your job names in the `jobs` table.
* **Customize Questions:**\
  You can freely add or remove questions. Just ensure each question ID is unique.
* **Check Database Setup:**\
  Make sure that the table name in `Config.Database.TableName` exists in your MySQL database.
