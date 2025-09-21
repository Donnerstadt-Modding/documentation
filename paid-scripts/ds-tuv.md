---
icon: car
---

# DS-TÜV

## DS-TÜV Script, Config Erklärung

Diese Seite beschreibt **jede Einstellung in `config.lua`**, ihre Bedeutung und mögliche Werte.

***

**Benötigt**

* ESXLegacy
* ox\_lib
* oxmysql
* ox\_target

***

**SQL-Eintragen (PFLICHT)**

```
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

***

### 1. Allgemeine Einstellungen

```lua
Config.DefaultLanguage = 'de'
Config.Debug = true
```

| Option            | Typ     | Beschreibung                                                                          | Beispiel |
| ----------------- | ------- | ------------------------------------------------------------------------------------- | -------- |
| `DefaultLanguage` | string  | Standard-Sprache des Scripts. Muss mit einer Sprachdatei (`de`, `en`) übereinstimmen. | `'de'`   |
| `Debug`           | boolean | Aktiviert Debug-Ausgaben auf dem Server. Nützlich, um Fehler zu erkennen.             | `true`   |

***

### 2. Datenbank Einstellungen

```lua
Config.Database = {
    TableName = "tuv_registrations"
}
```

| Option      | Typ    | Beschreibung                                                     | Beispiel              |
| ----------- | ------ | ---------------------------------------------------------------- | --------------------- |
| `TableName` | string | Name der MySQL-Tabelle, in der die TÜV-Daten gespeichert werden. | `"tuv_registrations"` |

⚠ **Hinweis:** Diese Option muss korrekt gesetzt sein, sonst gibt es SQL-Fehler.

***

### 3. Jobs

```lua
Config.Jobs = {
    TUVRegister = "rr",
    TUVCheck    = "rr"
}
```

| Option        | Typ    | Beschreibung                           | Beispiel     |
| ------------- | ------ | -------------------------------------- | ------------ |
| `TUVRegister` | string | Job, der **Fahrzeuge eintragen** darf. | `"mechanic"` |
| `TUVCheck`    | string | Job, der **nur TÜV prüfen** darf.      | `"mechanic"` |

> **Hinweis:** Beide Jobs können gleich sein, wenn ein Job alles darf.

***

### 4. TÜV Dauer Einstellungen

```lua
Config.Durations = {
    Allowed = {30, 60, 90, 120, 180},
    MaxMonths = 5
}
```

| Option      | Typ    | Beschreibung                                                     | Beispiel       |
| ----------- | ------ | ---------------------------------------------------------------- | -------------- |
| `Allowed`   | table  | Liste von Tagen, die als gültige TÜV-Dauer erlaubt sind.         | `{30, 60, 90}` |
| `MaxMonths` | number | Maximale Dauer in Monaten, die ein TÜV-Eintrag gültig sein darf. | `5`            |

***

### 5. TÜV Standort & Target

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

| Option                | Typ     | Beschreibung                                      | Beispiel                          |
| --------------------- | ------- | ------------------------------------------------- | --------------------------------- |
| `Position`            | vector3 | Koordinaten des festen TÜV-Punktes auf der Karte. | `vector3(212.79, -802.66, 30.86)` |
| `Target.Radius`       | number  | Radius der Interaktionszone.                      | `4.0`                             |
| `Target.Height`       | number  | Höhe der Interaktionszone.                        | `1.5`                             |
| `Target.Debug`        | boolean | Aktiviert Debug-Anzeige der Zone.                 | `true`                            |
| `Target.Options.Icon` | string  | Icon, das auf dem Zielpunkt angezeigt wird.       | `'fa-solid fa-car'`               |

***

### 6. TÜV Fragen

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
    {id = "q_suspension", required = false},
}
```

| Option     | Typ     | Beschreibung                                                     | Beispiel     |
| ---------- | ------- | ---------------------------------------------------------------- | ------------ |
| `id`       | string  | Eindeutige ID der Frage. Wird in der NUI und DB verwendet.       | `"q_brakes"` |
| `required` | boolean | Gibt an, ob die Frage **Pflicht** ist, um den TÜV abzuschließen. | `true`       |

> Tipp: Pflichtfragen müssen beantwortet werden, sonst wird der Eintrag verweigert.

***

### 7. Server Fehlermeldungen

```lua
Config.ServerMessages = {
    db_error_insert = 'Datenbankfehler beim Eintragen',
    db_error_delete = 'Datenbankfehler beim Löschen'
}
```

| Option            | Typ    | Beschreibung                                              | Beispiel                           |
| ----------------- | ------ | --------------------------------------------------------- | ---------------------------------- |
| `db_error_insert` | string | Meldung, wenn das Eintragen in die Datenbank fehlschlägt. | `"Datenbankfehler beim Eintragen"` |
| `db_error_delete` | string | Meldung, wenn das Löschen aus der Datenbank fehlschlägt.  | `"Datenbankfehler beim Löschen"`   |

***

### 8. Hinweise & Tipps

* **Debug aktivieren**: `Config.Debug = true` → Serverausgaben helfen bei Problemen.
* **Jobs prüfen**: Stelle sicher, dass `TUVRegister` und `TUVCheck` existieren und richtig gesetzt sind.
* **Fragen anpassen**: Du kannst beliebig viele Fragen hinzufügen oder entfernen, achte nur auf eindeutige IDs.
* **Datenbank prüfen**: `Config.Database.TableName` muss in MySQL existieren.
