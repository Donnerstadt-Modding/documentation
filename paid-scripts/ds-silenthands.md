---
icon: shield-quartered
---

# DS-SilentHands

**DS-SilentHands** ist ein FiveM Script, das Spieler kampfunfähig macht, die Steuerung einschränkt und einen Timer in einer Anzeige darstellt.\
Die Konfiguration erfolgt über `config.lua`. Jede Option ist unten detailliert erklärt.

***

**Benötigt**

* ESXLegacy
* ox\_inventory
* visn\_are

***

#### 1. Sprache der Anzeige

```lua
Config.Language = 'de'
```

**Beschreibung:** Legt die Sprache fest, die für die NUI-Anzeige verwendet wird.\
**Mögliche Werte:** `'de'`, `'en'` oder andere, die in deinem `locales` Ordner definiert sind.\
**Beispiel:**

```lua
Config.Language = 'en'  -- Anzeige auf Englisch
```

***

#### 2. Dauer der Kampfunfähigkeit

```lua
Config.IncapacitatedTime = 600
```

**Beschreibung:** Definiert die Zeit (in Sekunden), die ein Spieler kampfunfähig bleibt.\
**Beispiele:**

* `600` → 10 Minuten
* `300` → 5 Minuten

**Hinweis:** Wird automatisch als Timer in der NUI-Anzeige dargestellt.

***

#### 3. NUI-Anzeige Einstellungen

```lua
Config.NUI = {
    left = 20,
    top = 50,
    width = 200,
    showMinutesSeconds = true
}
```

**3.1 `left`**

Position der Statusbox von links in Pixeln.\
**Beispiel:** `left = 50` → 50 Pixel vom linken Bildschirmrand.

**3.2 `top`**

Vertikale Position der Statusbox in Prozent.\
**Beispiel:** `top = 50` → Zentriert auf dem Bildschirm.\
**Hinweis:** `0%` = oberer Bildschirmrand, `100%` = unterer Rand.

**3.3 `width`**

Breite der Statusbox in Pixeln.\
**Beispiel:** `width = 250` → Statusbox ist 250 Pixel breit.

**3.4 `showMinutesSeconds`**

Legt fest, wie der Timer angezeigt wird.\
**Werte:**

* `true` → MM:SS (Minuten:Sekunden)
* `false` → Nur Sekunden\
  **Beispiel:**

```lua
showMinutesSeconds = false
```

***

#### 4. Blockierte Controls

```lua
Config.BlockedControls = {
    24,  -- Attack
    25,  -- Aim
    47,  -- Weapon
    140, -- Melee Attack 1
    141, -- Melee Attack 2
    142, -- Melee Attack 3
    143, -- Melee Attack 4
    257, -- Attack 3
    263, -- Melee Attack 5
    264  -- Melee Attack 6
}

```

**Beschreibung:** Liste der Steuerungen, die während der Kampfunfähigkeit deaktiviert werden.

**Kontroll-IDs (Auswahl):**

* `24` → Attack
* `25` → Aim
* `47` → Weapon
* `140–143`, `257`, `263`, `264` → Verschiedene Nahkampfangriffe

**Hinweis:** Weitere Controls können hinzugefügt oder entfernt werden, je nach Bedarf.

***

#### 5. Erlaubte Waffen

```lua
Config.AllowedWeapons = {
    [GetHashKey("WEAPON_KNIFE")] = true
}
```

**Beschreibung:** Definiert, welche Waffen Spieler behalten dürfen, wenn sie kampfunfähig werden.

**Funktionsweise:** Alle nicht erlaubten Waffen werden automatisch entwaffnet.

**Beispiel:**

```lua
Config.AllowedWeapons = {
    [GetHashKey("WEAPON_KNIFE")] = true,
    [GetHashKey("WEAPON_PISTOL")] = true
}
```

**Hinweis:** Nutze `GetHashKey("WEAPON_NAME")` für jede erlaubte Waffe.

***

#### 6. Admin-Befehl: Kampfunfähigkeit beenden

Administratoren mit der `admin`-Group können Spieler aus dem kampfunfähigen Zustand befreien.\
**Command:**

```bash
/removeincap [playerId]
```

**Beschreibung:** Beendet die Kampfunfähigkeit bei einem bestimmten Spieler.\
**Beispiel:**

```bash
/removeincap 12
```
