> **Correction note (2026-07-03, DL-020):** This document used `project_id` and, inconsistently, `participant_id`. Standardised naming now in effect: `pid` = customer/cohort/programme run (was `project_id`), `user_id` = individual participant (was `participant_id` here, `pid` in the shipped Ready-Check code). All Fillout Hidden Fields and Zoho mappings need to be updated to match — this document's examples below still show the old names and need revision before reuse.

> **Correction note (2026-07-07, DL-023):** Ready Check no longer functions as a gate and no longer shares a `user_id`/pid-continuity relationship with the combined module. The connector logic below now applies to the combined module's Fillout submissions, and separately to Ready Check's own `pid`-only outcome submissions (see 15_Technical_Architecture.md, Ready-Check Tracking). Ready Check submissions do not carry a `user_id` field.

> **Correction note (2026-07-09, DL-025/DL-026):** A new routing-flag URL parameter, `stretch_relevant`, is introduced (see DL-025 for what it drives — the conditional Momentum expectation-violation question and Stretch-level selection — and DL-026 for where it is persisted, the Zoho Catalyst Resilience Layer). It is added to the parameter tables below using this document's existing (partly outdated, see the DL-020 correction note above) naming style; it should be read alongside the `pid`/`user_id` naming actually in force.

> **Correction note (2026-07-09, DL-027/DL-028):** Fillout is superseded by Zoho Forms as the form provider — see DL-027; update all Fillout-specific URLs, Hidden Field names and examples below accordingly before reuse. Separately, the SCORM/LMS delivery context assumed throughout (Rise 360 → SCORM → client LMS) is superseded for the MVP path by a self-hosted Rise 360 Web Export — see DL-028. The underlying URL-parameter-routing principle (Rise 360 → form, with pid/user_id/routing-flag parameters) remains valid and reusable for the Web Export path; the SCORM/LMS-specific framing does not.

---

# SCORMxFillout Connector – Script & Architecture Export

Stand: 2026-07-03  
Projektkontext: habify30 / Rise 360 / Fillout / Zoho

> Hinweis: Dieser Export rekonstruiert die implementierte Logik aus dem aktuellen Projektkontext. Gesichert ist: Fillout wird aus Rise 360 heraus mit URL-Parametern geöffnet; Fillout übernimmt Werte in Hidden Fields; zentrale Felder sind insbesondere `project_id` und ein Return-/Source-Flag. Wo konkrete Feldnamen kundenspezifisch variieren können, sind sie als konfigurierbare Parameter markiert.

---

## 1. Zweck der Connector-Logik

Der SCORMxFillout-Connector verbindet einen in Rise 360 / SCORM ausgelieferten Kurs mit einem externen Fillout-Formular.

Ziel ist nicht, personenbezogene Daten aus dem LMS technisch komplex auszulesen, sondern eine robuste und unternehmenskompatible Übergabe von Kontextdaten über URL-Parameter zu ermöglichen.

Typische Anwendungsfälle:

- Ein Teilnehmer klickt im Rise-360-Kurs auf einen Link oder Button.
- Das Fillout-Formular öffnet sich in einem neuen Tab.
- Das Formular erhält automatisch technische Kontextwerte, z. B.:
  - `project_id`
  - `source`
  - `return_to_course`
  - optional `participant_id`
  - optional `cohort_id`
- Fillout schreibt diese Werte in Hidden Fields.
- Nach Submission können diese Werte in Zoho Flow / Zoho CRM / Zoho Analytics weiterverarbeitet werden.

---

## 2. Leitprinzipien

### 2.1 Keine unnötige Komplexität

Die bevorzugte Lösung nutzt URL-Parameter statt JavaScript-Injektion.

Vorteile:

- sehr geringe Blockierungswahrscheinlichkeit durch Unternehmensrichtlinien
- kein externes Script
- keine Third-Party-Assets
- keine Abhängigkeit von Browser-Sicherheitsfeatures
- stabil in LMS-/SCORM-Umgebungen
- gut prüfbar und dokumentierbar

### 2.2 Fillout übernimmt die Hidden-Field-Befüllung

Fillout kann Hidden Fields über URL-Parameter vorbefüllen.

Beispiel:

```text
https://form.fillout.com/t/vBbNMysv7ous?project_id=D_FAB_25_001
```

Wenn im Fillout-Formular ein Hidden Field mit passender Parameterlogik existiert, wird `D_FAB_25_001` gespeichert.

---

## 3. Minimaler Connector: Rise 360 → Fillout

### 3.1 Statischer Projektlink

Für einen festen Projektdurchlauf:

```text
https://form.fillout.com/t/vBbNMysv7ous?project_id=D_FAB_25_001
```

### 3.2 Mit Return-/Source-Flag

```text
https://form.fillout.com/t/vBbNMysv7ous?project_id=D_FAB_25_001&source=rise360&return_to_course=true
```

### 3.3 Empfohlene Standardparameter

| Parameter | Beispiel | Zweck |
|---|---:|---|
| `project_id` | `D_FAB_25_001` | Eindeutige Projekt-/Programmkennung |
| `source` | `rise360` | Herkunft des Formularaufrufs |
| `return_to_course` | `true` | Markiert, dass User aus Kurskontext kommt |
| `cohort_id` | `C01` | Optional: Kohorte |
| `lang` | `de` | Optional: Sprache |
| `step` | `ready_check` | Optional: Prozessschritt |
| `stretch_relevant` | `true` | Routing-Flag aus der Veränderungswerkstatt; steuert die bedingte Momentum-Reflexionsfrage und die Stretch-Stufen-Auswahl (siehe DL-025, DL-026) |

---

## 4. HTML-Link für Rise 360 / LMS-Kontext

Falls ein HTML-Block oder LMS-Beschreibung genutzt wird:

```html
<a href="https://form.fillout.com/t/vBbNMysv7ous?project_id=D_FAB_25_001&source=rise360&return_to_course=true"
   target="_blank"
   rel="noopener noreferrer">
   Formular öffnen
</a>
```

---

## 5. Optionales JavaScript: URL dynamisch zusammenbauen

Nur verwenden, wenn in der konkreten Umgebung JavaScript zugelassen ist.  
Für maximale Unternehmenskompatibilität ist die statische URL vorzuziehen.

```html
<script>
(function () {
  const filloutBaseUrl = "https://form.fillout.com/t/vBbNMysv7ous";

  const params = new URLSearchParams({
    project_id: "D_FAB_25_001",
    source: "rise360",
    return_to_course: "true"
  });

  const targetUrl = filloutBaseUrl + "?" + params.toString();

  const link = document.getElementById("open-fillout");
  if (link) {
    link.href = targetUrl;
    link.target = "_blank";
    link.rel = "noopener noreferrer";
  }
})();
</script>

<a id="open-fillout" href="#">Formular öffnen</a>
```

---

## 6. Optional: Parameter aus aktueller URL übernehmen

Diese Variante ist nützlich, wenn Rise/LMS selbst Parameter erhält und an Fillout weiterreichen soll.

Beispiel:  
Rise-Seite wird aufgerufen mit:

```text
...?project_id=D_FAB_25_001&participant_id=abc123
```

Connector:

```html
<script>
(function () {
  const filloutBaseUrl = "https://form.fillout.com/t/vBbNMysv7ous";
  const incoming = new URLSearchParams(window.location.search);

  const allowedKeys = [
    "project_id",
    "participant_id",
    "cohort_id",
    "lang"
  ];

  const outgoing = new URLSearchParams();

  allowedKeys.forEach(function (key) {
    const value = incoming.get(key);
    if (value) {
      outgoing.set(key, value);
    }
  });

  outgoing.set("source", "rise360");
  outgoing.set("return_to_course", "true");

  const targetUrl = filloutBaseUrl + "?" + outgoing.toString();

  const link = document.getElementById("open-fillout");
  if (link) {
    link.href = targetUrl;
    link.target = "_blank";
    link.rel = "noopener noreferrer";
  }
})();
</script>

<a id="open-fillout" href="#">Formular öffnen</a>
```

---

## 7. Fillout-Konfiguration

Im Fillout-Formular werden Hidden Fields angelegt.

Empfohlene Hidden Fields:

| Hidden Field | URL-Parameter | Beispielwert |
|---|---|---|
| `project_id` | `project_id` | `D_FAB_25_001` |
| `source` | `source` | `rise360` |
| `return_to_course` | `return_to_course` | `true` |
| `cohort_id` | `cohort_id` | `C01` |
| `participant_id` | `participant_id` | `abc123` |
| `stretch_relevant` | `stretch_relevant` | `true` |

Wichtig: Die Namen müssen konsistent zwischen URL-Parameter, Fillout Hidden Field und späterem Zoho Mapping sein.

---

## 8. Datenfluss

```text
Rise 360 / SCORM
   ↓
Button oder Link mit URL-Parametern
   ↓
Fillout Form
   ↓
Hidden Fields speichern Kontextwerte
   ↓
Fillout Submission
   ↓
ggf Zoho Flow / Sheet


---

## 9. Warum `project_id` zentral ist

Die `project_id` ist der technische Schlüssel für eine konkrete Programmdurchführung.

Sie darf nicht mit dem Programmtyp verwechselt werden:

| Ebene | Beispiel | Bedeutung |
|---|---|---|
| Programmtyp | `habify30` | Produkt-/Formatlogik |
| Project-ID | `26-DEU-WEN-003` | konkrete Durchführung |
| Teilnahme | E-Mail + Project-ID | konkrete Teilnahme an Durchführung |

Damit können Formulare, Kampagnen, Reports und CRM-Datensätze eindeutig einem Programmdurchlauf zugeordnet werden.

---

## 10. Sicherheits- und DSGVO-Logik

### 10.1 Keine sensiblen Daten in der URL

In URL-Parametern sollten keine sensiblen personenbezogenen Daten stehen.

Geeignet:

- `project_id`
- `cohort_id`
- `source`
- `lang`
- pseudonyme IDs

Nicht empfohlen:

- Klarnamen
- private E-Mail-Adressen, wenn vermeidbar
- Gesundheitsdaten
- Antworten aus Assessments
- sensible Freitextangaben

### 10.2 Personenbezug minimieren

Wenn Teilnehmerdaten nur für die Programmdurchführung genutzt werden dürfen, sollte `project_id` genutzt werden, um den Programmkontext herzustellen, ohne automatisch einen Marketing-Kontakt zu erzeugen.

---

## 11. Empfohlene Benennung

### URL-Parameter

```text
project_id
source
return_to_course
cohort_id
participant_id
lang
step
stretch_relevant
```

### Fillout Hidden Fields

Gleichnamig zu den URL-Parametern.

### Zoho-Felder

| Zoho-Modul | Feld |
|---|---|
| Programme | `Project_ID` |
| Teilnahmen | `Project_ID_Text` oder Lookup → Programme |
| Teilnahmen | `Source` |
| Teilnahmen | `Return_To_Course` |
| Teilnahmen | `Cohort_ID` |
| Teilnahmen | `Participant_ID` |
| Teilnahmen | `Stretch_Relevant` — nicht in Zoho CRM/Analytics, sondern im Zoho Catalyst Resilience Layer (siehe DL-026, 15_Technical_Architecture.md) |

---

## 12. Testfälle

### Test 1: Project-ID wird übernommen

URL:

```text
https://form.fillout.com/t/vBbNMysv7ous?project_id=D_FAB_25_001
```

Erwartung:

- Fillout Submission enthält `project_id = D_FAB_25_001`.

### Test 2: Source wird übernommen

URL:

```text
https://form.fillout.com/t/vBbNMysv7ous?project_id=D_FAB_25_001&source=rise360
```

Erwartung:

- Fillout Submission enthält `source = rise360`.

### Test 3: Return-Flag wird übernommen

URL:

```text
https://form.fillout.com/t/vBbNMysv7ous?project_id=D_FAB_25_001&return_to_course=true
```

Erwartung:

- Fillout Submission enthält `return_to_course = true`.

---

## 13. Minimal empfohlene Produktivlösung

Für höchste Stabilität:

```text
https://form.fillout.com/t/vBbNMysv7ous?project_id=26-DEU-WEN-003&source=rise360&return_to_course=true
```

Diese Lösung ist ohne Script, ohne externe Ressourcen und damit am wenigsten anfällig für Blockaden durch Unternehmensrichtlinien.

---

## 14. Offene Konfigurationspunkte

Vor Livegang je Kunde prüfen:

- finale `project_id`
- korrekter Fillout-Link
- Hidden Fields im Fillout-Formular vorhanden
- Zoho Flow Mapping korrekt
- Testsubmission durchgeführt
- keine personenbezogenen Daten unnötig in URL-Parametern
- Lösch-/Aufbewahrungslogik für Teilnehmerdaten definiert
