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

Dieses Script ermöglicht die **Fahrzeug-Zulassung** über Jeden Beibiegen Job.\
Damit alles richtig funktioniert, musst du die `config.lua` anpassen.\
Hier erklären wir **jede einzelne Einstellung** im Detail.

***

### Benötigt

* ESXLegacy
* ox\_target
* (Optional) lation\_ui

***

### Sprache einstellen

```lua
Config.Locale = "de"
```

* `"de"` → Deutsch
* `"en"` → Englisch

> 💡 Tipp: Falls du weitere Sprachen hinzufügen möchtest, nutze die Datei `translation.lua`.

***

### Jobs konfigurieren

```lua
Config.JobPolice = "polizei"       -- Polizei-Job
Config.JobRoadRescue = "rr"        -- RoadRescue-Job
```

* **JobPolice** → Name des Polizei-Jobs in deiner Datenbank
* **JobRoadRescue** → Name des RoadRescue-Jobs (z. B. Mechaniker oder Stadtwerke)

> ⚠️ Stelle sicher, dass diese Namen **exakt** mit deinen Job-Namen in `jobs` übereinstimmen.

***

### Datenbank-System wählen

```lua
Config.DB = "oxmysql"
```

Unterstützt werden:

* `"oxmysql"` (empfohlen ✅)
* `"ghmattimysql"`
* `"mysql-async"`

> 🔧 Wähle das System, das dein Server nutzt.

***

### Interaktionen

```lua
Config.InteractionDistance = 3.0
Config.RegistrationPoint = vec3(-303.1077, -1386.8650, 31.4423)
```

* **InteractionDistance** → Wie nah Spieler am Fahrzeug stehen müssen (in Metern).
* **RegistrationPoint** → Koordinaten des Zulassungspunkts (meistens beim Mechaniker oder bei den Stadtwerken).

> 💡 Du kannst die Koordinaten mit einem Ingame-Tool (z. B. `/coords`) anpassen.

***

### Benachrichtigungen

```lua
Config.NotificationSystem = "esx" 
```

Mögliche Systeme:

* `"esx"` → Standard ESX Notification
* `"ox_lib"` → Modernes Notification-System (nutzt `ox_lib`)
* `"custom"` → Eigenes System (muss in `Notify()` definiert werden)

***

#### Optionen für ox\_lib

```lua
Config.LibNotify = {
    title = "Fahrzeug System",
    type = "success",   -- success | error | info
    duration = 5000
}
```

* **title** → Überschrift der Nachricht
* **type** → Art der Nachricht (`success`, `error`, `info`)
* **duration** → Dauer in Millisekunden (z. B. 5000 = 5 Sekunden)

***

### Custom Notify-Funktion

Wenn du `custom` auswählst, kannst du im `Notify(msg)`-Block **dein eigenes System** einfügen.\
Beispiel:

```lua
elseif Config.NotificationSystem == "custom" then
    TriggerEvent("myCustomNotify", msg)
end
```

***

### Checkliste nach Anpassung

1. `config.lua` speichern
2. Script neu starten
3. Testen:
   * Kann Der Mechaniker Job Fahrzeuge registrieren?
   * Werden Benachrichtigungen korrekt angezeigt?
   * Funktioniert der Zulassungspunkt?

***

> 🚨 **Hinweis:**\
> Ohne korrekt gesetzte Config läuft das Script nicht richtig.\
> Besonders die Jobnamen, das Datenbanksystem und die Koordinaten müssen stimmen!
