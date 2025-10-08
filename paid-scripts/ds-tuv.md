---
icon: car
---

# DS-TÜV

## DS-TÜV Script – Config Explanation

This page explains every setting in the `config.lua` file, including its purpose and possible values.

***

### Requirements

* ESX Legacy
* ox\_lib
* oxmysql
* ox\_target

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
Config.DefaultLanguage = 'de'
Config.Debug = true
```

**Description:**\
Defines the default language and enables debug messages in the server console.

**Values:**

* `Config.DefaultLanguage` → `'de'` for German, `'en'` for English
* `Config.Debug` → `true` (shows debug logs), `false` (silent mode)

***

### 2. Database Settings

```lua
Config.Database = {
    TableName = "tuv_registrations"
}
```

**Note:**\
This setting **must match your SQL table name** exactly.\
If incorrect, SQL errors will occur during registration or deletion.

***

### 3. Jobs

```lua
Config.Jobs = {
    TUVRegister = "rr",
    TUVCheck    = "rr"
}
```

**Description:**\
Defines which job roles can register or check TÜV status.

**Note:**\
Both jobs can be the same if a single job should handle both actions (for example, a mechanic or road rescue department).

***

### 4. TÜV Duration Settings

```lua
Config.Durations = {
    Allowed = {30, 60, 90, 120, 180},
    MaxMonths = 5
}
```

**Description:**\
Defines the allowed TÜV durations in days and sets a maximum duration limit (in months).

* `Allowed` → List of valid durations (in days)
* `MaxMonths` → Maximum allowed duration in months

***

### 5. TÜV Location & Target

```lua
Config.Location = {
    Position = vector3(212.7984, -802.6668, 30.8652),
    Target = {
        Radius  = 4.0,
        Height  = 1.5,
        Debug   = true,
        Options = {
            Icon = 'fa-solid fa-car'
        }
    }
}
```

**Description:**\
Defines where the TÜV registration point is located and how it interacts with ox\_target.

* `Position` → Coordinates of the TÜV location
* `Radius` → Interaction radius around the point
* `Height` → Vertical detection range
* `Debug` → Shows interaction markers (for testing)
* `Icon` → Displayed icon for ox\_target

***

### 6. TÜV Questions

```lua
Config.Questions = {
    {id = "q_brakes", required = true},
    {id = "q_lights", required = true},
    {id = "q_tires", required = true},
    {id = "q_emissions", required = false},
    {id = "q_oil", required = false},
    {id = "q_wipers", required = false},
    {id = "q_wheels", required = false},
    {id = "q_horn", required = false},
    {id = "q_windows", required = false},
    {id = "q_suspension", required = false}
}
```

**Description:**\
Defines the list of questions displayed during the TÜV inspection.

**Tip:**\
All questions marked as `required = true` must be answered.\
If any required question is left unanswered, the vehicle will not pass inspection.

***

### 7. Server Messages

```lua
Config.ServerMessages = {
    db_error_insert = 'Database error during registration',
    db_error_delete = 'Database error during deletion'
}
```

**Description:**\
Defines custom messages for server console or client notifications when database errors occur.

***

### 8. Tips & Recommendations

* **Enable Debug Mode:**\
  Set `Config.Debug = true` to view helpful console logs for troubleshooting.
* **Verify Job Names:**\
  Ensure that `TUVRegister` and `TUVCheck` exist and match your job names in the `jobs` table.
* **Customize Questions:**\
  You can freely add or remove questions. Just ensure each question ID is unique.
* **Check Database Setup:**\
  Make sure that the table name in `Config.Database.TableName` exists in your MySQL database.
