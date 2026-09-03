> **Correction note (2026-07-03, DL-020):** This document previously used `user_id` for the participant-level identifier and described `project_id` for the cohort-level identifier. Standardised naming now in effect: `pid` = customer/cohort/programme run, `user_id` = individual participant. `project_id` below should be read as `pid` throughout; the participant identifier naming (`user_id`, `habify_user_id`) already matches the new standard and needs no change.

> **Correction note (2026-07-07, DL-023):** The Ready Check no longer functions as a technical gate and no longer requires `user_id` continuity into the combined module. The architecture described below now applies only within the combined Impulsphase/Veränderungswerkstatt/Momentum module, which is a single SCORM package and does not cross an origin boundary internally. See DL-023 and 15_Technical_Architecture.md.

> **Correction note (2026-07-09, DL-026):** The `user_id` generation, storage and recovery mechanics described below (sections 3–9: `localStorage`-only persistence, no key-versioning, no recovery flow) are superseded. `user_id` persistence is now primarily via encrypted, key-versioned `cmi.suspend_data`, with `localStorage` retained only as a same-session cache and a Zoho Catalyst recovery-code flow as fallback — designed to survive restrictive corporate IT environments and worst-case SCORM 1.2 LMS behaviour, without introducing login/accounts. Full mechanics: see DL-026 and 15_Technical_Architecture.md, "Resilience & Recovery Architecture — Decided (DL-026)". Not duplicated here. UID generation is also no longer automatic on first load (section 4 below) — it now happens exactly once, triggered by an explicit user action early in the Impulsphase (DL-026).

> **Correction note (2026-07-09, DL-027/DL-028):** Fillout (referenced throughout the examples below) is superseded by Zoho Forms as the form provider — see DL-027; the URL-parameter-routing principle shown is unaffected in concept. Separately, and more substantially, the SCORM-specific delivery context assumed throughout this document (SCORM export, SCORM package, LMS delivery) is itself superseded for the MVP path: habify30 now ships via a self-hosted Rise 360 Web Export, not SCORM/LMS delivery — see DL-028 and 15_Technical_Architecture.md, "Resilience & Recovery Architecture for Web Export." This document is retained as reference for a possible future SCORM custom build, not as the active current architecture.

---

# SCORMxFillout – Projekt-ID (pid) & User-ID Architektur

Stand: 2026-07-03

> Dieses Dokument beschreibt die Architekturidee hinter der Übergabe der `project_id`
> sowie der pseudonymen `user_id`. Es beschreibt die beabsichtigte Implementierung
> und dient als Referenz für den Neuaufbau.

---

# 1. Grundprinzip

Der Teilnehmer soll **keine technischen Informationen eingeben müssen**.

Insbesondere werden **Project-ID** und **User-ID** vollständig im Hintergrund erzeugt
bzw. weitergegeben.

Der Teilnehmer sieht davon nichts.

---

# 2. Project-ID

## Ziel

Jede Programmdurchführung besitzt eine eindeutige Project-ID.

Beispiel

26-DEU-WEN-003

Diese Project-ID identifiziert

- den Kunden
- den Programmdurchlauf
- alle Formulare
- alle Kampagnen
- sämtliche Analytics-Auswertungen

---

## Speicherung im SCORM

Für jede veröffentlichte SCORM-Version wird die Project-ID **vor dem Export**
fest in der HTML-Datei hinterlegt.

Beispiel:

```javascript
const PROJECT_ID = "26-DEU-WEN-003";
```

Alternativ kann sie in einer kleinen Konfigurationsdatei stehen:

```javascript
window.HABIFY_CONFIG = {
    project_id: "26-DEU-WEN-003"
};
```

Dadurch ist keinerlei Benutzereingabe erforderlich.

---

## Verwendung

Beim Öffnen eines Fillout-Links wird diese Konstante automatisch
als URL-Parameter angehängt.

Beispiel

```
https://form.fillout.com/...
    ?project_id=26-DEU-WEN-003
```

Fillout schreibt den Wert anschließend in ein Hidden Field.

---

# 3. User-ID

## Ziel

Der Benutzer erhält eine stabile, pseudonyme Kennung.

Diese Kennung dient ausschließlich dazu,

- mehrere Formularaufrufe derselben Person
- mehrere Assessments
- mehrere Kursabschnitte

innerhalb desselben Browsers zusammenzuführen.

Es handelt sich **nicht** um eine personenbezogene Kennung.

---

# 4. Erzeugung

Beim ersten Öffnen des SCORM wird geprüft:

```
Existiert bereits eine User-ID?
```

Falls nein:

```
UUID erzeugen
```

Beispiel

```
4b39d5d6-a904-4bcb-b0b4-c68d71395e5d
```

---

# 5. Speicherung

Die User-ID wird im Browser gespeichert.

Empfohlen:

```
localStorage
```

Beispiel

```javascript
localStorage.setItem("habify_user_id", uuid);
```

Beim nächsten Öffnen wird dieselbe ID wiederverwendet.

```javascript
const id = localStorage.getItem("habify_user_id");
```

---

# 6. Warum kein Session Cookie?

Während der Architekturdiskussion wurden Session-Cookies erwogen.

Die bevorzugte Lösung ist jedoch:

**localStorage**

Gründe:

- überlebt Browser-Neustarts
- keine Serverkonfiguration
- keine Cookie-Banner-Problematik
- einfacher Zugriff aus JavaScript
- stabil für SCORM-Inhalte

Ein Session-Cookie würde beim Schließen des Browsers verloren gehen.

---

# 7. Algorithmus

Pseudo-Code

```text
Beim Laden des Kurses

↓

PROJECT_ID aus Konfiguration lesen

↓

localStorage prüfen

↓

User-ID vorhanden?

Ja
↓

verwenden

Nein
↓

UUID erzeugen

↓

im localStorage speichern

↓

Fillout-Link erzeugen

↓

project_id anhängen

↓

user_id anhängen

↓

Formular öffnen
```

---

# 8. Beispielcode

```javascript
const PROJECT_ID = "26-DEU-WEN-003";

let userId = localStorage.getItem("habify_user_id");

if (!userId) {
    userId = crypto.randomUUID();
    localStorage.setItem("habify_user_id", userId);
}

const url =
"https://form.fillout.com/t/vBbNMysv7ous"
+ "?project_id=" + encodeURIComponent(PROJECT_ID)
+ "&user_id=" + encodeURIComponent(userId)
+ "&source=rise360";

window.open(url, "_blank");
```

---

# 9. Datenfluss

```
SCORM

↓

PROJECT_ID aus Konfiguration

↓

User-ID aus localStorage

↓

Fillout

↓

Hidden Fields

↓

Zoho Flow

↓

Analytics
```

---

# 10. Datenschutz

Die User-ID ist ausschließlich ein technischer,
pseudonymer Identifier.

Sie enthält

- keinen Namen
- keine E-Mail
- keine personenbezogenen Informationen

Eine Zuordnung zu einer Person erfolgt ausschließlich,
wenn der Benutzer diese Daten später freiwillig in einem Formular angibt.

Damit bleiben Kursnavigation und Formularfluss technisch verknüpfbar,
ohne personenbezogene Daten dauerhaft im Browser zu speichern.
