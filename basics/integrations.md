---
icon: paste
layout:
  width: default
  title:
    visible: true
  description:
    visible: true
  tableOfContents:
    visible: true
  outline:
    visible: true
  pagination:
    visible: true
  metadata:
    visible: true
---

# DS-Registrations

## DS-Registration

This script enables vehicle registration through any selected job (e.g. Police, Mechanic, or Road Rescue).\
To ensure everything works correctly, you must configure the `config.lua` file.\
This guide explains every setting in detail.

***

### Requirements

* ESX Legacy
* ox\_target
* (Optional) lation\_ui

***

### Set Language

```lua
Config.Locale = "de"
```

Available options:\
`"de"` → German\
`"en"` → English

Tip: To add more languages, use the `translation.lua` file.

***

### Configure Jobs

```lua
Config.JobPolice = "polizei"       -- Police job
Config.JobRoadRescue = "rr"        -- Road Rescue job
```

* **JobPolice** → Name of the police job in your database.
* **JobRoadRescue** → Name of the Road Rescue or mechanic job.

Make sure these names **exactly match** the ones in your job database.

***

### Select Database System

```lua
Config.DB = "oxmysql"
```

Supported options:

* `"oxmysql"` (recommended)
* `"ghmattimysql"`
* `"mysql-async"`

Choose the system your server currently uses.

***

### Interactions

```lua
Config.InteractionDistance = 3.0
Config.RegistrationPoint = vec3(-303.1077, -1386.8650, 31.4423)
```

* **InteractionDistance** → How close players must be to the vehicle (in meters).
* **RegistrationPoint** → Coordinates of the registration point (e.g. mechanic or city service location).

You can adjust the coordinates with an in-game tool such as `/coords`.

***

### Notifications

```lua
Config.NotificationSystem = "esx"
```

Available systems:

* `"esx"` → Default ESX notifications
* `"ox_lib"` → Modern notification system using ox\_lib
* `"custom"` → Your own notification handler (defined in `Notify()`)

***

### Options for ox\_lib

```lua
Config.LibNotify = {
    title = "Vehicle System",
    type = "success",   -- success | error | info
    duration = 5000
}
```

* **title** → Notification title
* **type** → Type of notification (success, error, info)
* **duration** → Duration in milliseconds (e.g. 5000 = 5 seconds)

***

### Custom Notify Function

If you choose `"custom"`, define your own notification event inside the `Notify(msg)` block.

Example:

```lua
elseif Config.NotificationSystem == "custom" then
    TriggerEvent("myCustomNotify", msg)
end
```

***

### Post-Setup Checklist

1. Save the `config.lua` file.
2. Restart the script.
3. Test the following:
   * Can the mechanic (or assigned job) register vehicles?
   * Are notifications displayed correctly?
   * Does the registration point function as intended?

***

### Important Note

The script will not function correctly without proper configuration.\
Ensure the following are set correctly:

* Job names
* Database system
* Registration coordinates
