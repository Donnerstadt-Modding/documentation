---
icon: toolbox
---

# DS-Repairkit

## DS-RepairKit, Konfiguration & Anleitung

Diese Seite erklärt, wie du **DS-RepairKit** konfigurierst und verwendest.

***

### Benötigt

* ESXLegacy
* ox\_target
* (Optional) lation\_ui

***

### Konfigurationsoptionen

Alle Einstellungen befinden sich in der Datei `config.lua`.

#### Job-Einstellungen

```lua
Config.AllowedJob = "rr"          -- Job, der das RepairKit verwenden darf
Config.MaxAllowedOnDuty = 1       -- Maximale Anzahl Spieler mit diesem Job gleichzeitig im Dienst
```

#### Reparatur-Einstellungen

```lua
Config.RepairDuration = 5000      -- Dauer der Reparatur in Millisekunden
Config.RepairItem = "repair_kit"  -- Name des Items, das zum Reparieren benötigt wird
```

***

### Lokalisierung / Texte

```lua
Config.Locales = {
    ["not_allowed"] = "Zu viele Personen mit dem Job '%s' sind bereits im Dienst!",
    ["used_repairkit"] = "Du hast ein Reparaturkit benutzt.",
    ["repairing"] = "Fahrzeug wird repariert...",
    ["no_vehicle"] = "Kein Fahrzeug in der Nähe.",
    ["cancelled"] = "Reparatur abgebrochen.",
    ["no_kit"] = "Du hast kein Reparaturkit dabei.",
    ["fixed"] = "Fahrzeug wurde repariert.",
    ["fixcar"] = "Fahrzeug reparieren"
}
```

* Passe die Texte an, um sie in deiner gewünschten Sprache zu nutzen.

***

### Benachrichtigungen

```lua
Config.NotifyType = "ox" -- Optionen: ox, esx, custom
```

Die Funktion `Notify(msg, type)` sendet Nachrichten an den Spieler:

```lua
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

* **ox** → Verwendet `lib.notify`
* **esx** → Verwendet `ESX.ShowNotification`
* **custom** → Ermöglicht eigene Notification Events

***

### Installation

1. Script in deinen Serverordner kopieren (`resources/DS-Repairkit`)
2.  In der `server.cfg` eintragen:

    ```
    ensure DS-Repairkit
    ```
3. `config.lua` nach Wunsch anpassen
4. Benötigte Items (`repair_kit`) im Inventarsystem hinzufügen
5. Server starten und testen

***

### Tipps

* Stelle sicher, dass der Job in `Config.AllowedJob` existiert.
* Passe `Config.RepairDuration` an, um realistische Reparaturzeiten zu setzen.
* Nutze `Config.NotifyType`, um die Benachrichtigungen an dein Serversystem anzupassen.
* Übersetze die `Locales` für andere Sprachen, falls nötig.
