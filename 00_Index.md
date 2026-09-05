# 00_Index.md — Themen-Index zum Decision Log

**Stand:** 2026-09-05 (Decision Log bei DL-082)
**Zweck:** Vor jeder Arbeit an einem Thema hier nachsehen, welche DL-Einträge es berühren. Erst dann bauen.

Dieser Index existiert, weil in der Session vom 2026-07-14 eine Peergruppen-Seite ohne Consent-Checkbox und ohne Domain-Validierung gebaut wurde — beides war seit **DL-036** spezifiziert. Der Fehler war nicht, dass die Doku schlecht organisiert war. Der Fehler war, dass nicht nachgesehen wurde.

**Pflege:** Bei jedem neuen DL-Eintrag hier ergänzen. Der Index ist wertlos, sobald er hinterherhinkt.

---

## Zugang, Identität, Wiederherstellung

| DL | Inhalt |
|---|---|
| **DL-020** | Namenskonvention: `pid` = Kunde/Kohorte/Programmlauf · `user_id` = einzelner Teilnehmer |
| **DL-026** | Persistenz- und Pseudonymitäts-Architektur für `user_id` · *Korrekturnotiz durch DL-059 (uid-Erzeugung nach Wizard Schritt 2 verschoben)* |
| **DL-028** | Wechsel zu selbstgehostetem Web Export · Catalyst-Backend · pid-Whitelist · Recovery-Code-Mechanismus |
| **DL-029** | `accesscontrol` und `recovery` als zwei getrennte, entkoppelte Catalyst-Funktionen · *Korrekturnotizen durch DL-042, DL-057 (Recovery-Pfad: `/recover` zuerst, `accesscontrol` nachgelagert), DL-058 (Response-Shape erweitert)* |
| **DL-031** | `pid` darf in `localStorage` gecacht werden (verfeinert DL-028) · *Korrekturnotizen durch DL-057 (Sequenzregel gilt nicht für Recovery-Pfad), DL-062 (Hard-Lock-Fall ersetzt durch Fehlerseite Zustand F)* |
| **DL-042** | Recovery-Code-Eingabe: **ein** Textfeld, keine acht Boxen · PDF + mailto, **kein „Kopieren"** · *Korrekturnotiz durch DL-064 (eingefrorene „Auch wir nicht"-Copy war an drei von drei Stellen aufgeweicht)* |
| **DL-051** | **Wizard:** 3 Schritte · `wizardCompleted` als lokales Flag · Sprache via `navigator.language` · *Korrekturnotizen durch DL-059 (uid-Erzeugung in Schritt 2), DL-060 (Zwangswahl → beobachtete Handlung + Kaskade), DL-061 (Home-Prompt entfällt), DL-066 (H1/Intro Schritt 1 neu gefasst), DL-064 (eingefrorene Copy wiederholt aufgeweicht)* |
| **DL-055** | `Einstieg`-Screen vor dem Wizard — erscheint nur ohne uid im localStorage · zwei CTAs: „Zugang vorhanden" → Code eingeben · „Neu hier" → Wizard · *Korrekturnotiz durch DL-063 (Reihenfolge gedreht, beide gleichrangig, Wizard-1-Rückweg als Bedingung)* |
| **DL-063** | `Einstieg`: **„Neu hier" oben, beide Optionen gleichrangig (Secondary)** · Wizard-Schritt-1-Rückweg („Wiederherstellungscode eingeben") als tragende Bedingung · Icons `flag` / `rotate-ccw` in `text/muted` · korrigiert DL-055 |
| **DL-066** | Wizard Schritt 1: H1 trägt das mentale Modell (**„Kein Passwort, keine Anmeldung"**), nicht die Begrüßung · korrigiert DL-051 |
| **DL-056** | `Einstieg — Code eingeben` — eigener Screen mit Recovery-Code-Feld · kein „Neu anfangen"-Button |
| **DL-057** | Recovery-Pfad: `/recover` zuerst (liefert `pid` mit), danach `accesscontrol(pid)` · Rate-Limit auf `/recover` nicht optional |
| **DL-058** | `accesscontrol` Response-Shape erweitert: `reason`, `expiryDate`, `programmName`, `contactEmail` |
| **DL-059** | uid-Erzeugung von der Impulsphase nach **Wizard Schritt 2** verschoben · Schritt 2 muss idempotent sein · Seat-Zählung ab jetzt: Wizard-Abschlüsse |
| **DL-081** | **State-/Speicher-Vertrag:** ein gebündelter `localStorage`-Store `h30.state` (pid, userId, recoveryCode, wizardCompleted, language, reservierte `progress`/`ui`) mit `schemaVersion` · Autoritätsmodell (localStorage = Gerätewahrheit, Catalyst = Backend) · Server-Antwort-Shapes (accesscontrol/register/recover) · sensible Zonen (Coach-RAM DL-072, Themenlabels server/uid DL-073) außerhalb des Stores |

**Vor dem Bau von:** Einstieg, Einstieg Code eingeben, Fehlerseite, Wizard, Einstellungen, Gerät verknüpfen, Recovery-Screens. **DL-081 ist Pflichtlektüre für jeden Screen, der Identität, Fortschritt oder das Capabilities-Objekt anfasst.**

---

## Peergruppen

| DL | Inhalt |
|---|---|
| **DL-010** | Peer-Interaktion ist Teil der Transfer-Architektur |
| **DL-011** | Peer-Strukturen bleiben unterstützend, nicht bewertend |
| **DL-035** | Gruppenbildung: 2–3 Personen · Stichtag während der Werkstattphase · **vollständig zufällige Zuteilung** |
| **DL-036** | **Anmeldung: Consent-Checkbox (Pflicht, aktiv) + E-Mail-Domain-Validierung.** `allowedEmailDomains` als **Array** (Konzerntöchter) · `manualDomainExceptions` für Externe. Erster Punkt im Produkt, an dem identifizierende Daten preisgegeben und mit anderen Teilnehmern geteilt werden |
| **DL-037** | Selbst-Austritt aus der Gruppe · **verbleibende Mitglieder werden per E-Mail informiert** · *Korrekturnotiz durch DL-041* |
| **DL-053** | **Drei eigenständige pid-only-Seiten** (Anmeldung · Austritt Schritt 1 · Austritt Schritt 2) · **Shell weiß nicht, ob jemand in einer Gruppe ist** · kein `peerGroupId`-Flag an der uid · Austrittsbestätigung per E-Mail nicht verhandelbar |

**Vor dem Bau von:** allem, was mit Peergruppen zu tun hat. DL-036 ist die Falle — Consent und Domain-Validierung sind Pflicht, nicht optional. DL-053: Shell verlässt die uid bei allem Peergruppen-bezogenen.

---

## Momentum-Phase, Cueing, Erinnerungen

| DL | Inhalt |
|---|---|
| **DL-018** | Erfolg = Unabhängigkeit von der Plattform · *Korrekturnotiz durch DL-040* |
| **DL-019** | **Kein digitaler Reminder-Kanal** in der Momentum-Phase |
| **DL-040** | Korrigiert DL-019: Peer-Interaktion ist der **primäre**, nicht der **einzige** Cueing-Mechanismus. Reine Wortkorrektur, **keine** personalisierten Reminder |

**Vor dem Bau von:** allem, was nach Benachrichtigung, Erinnerung oder Push aussieht. Die Grenze ist scharf.

---

## Navigation, Shell, Home

| DL | Inhalt |
|---|---|
| **DL-030** | Shell-Architektur · Phasen-Freischaltung per Datum/Fortschritt, Shell-Routing erzwingt Gate *(Rise/iframe-Mechanik durch DL-076 ersetzt — Korrekturnotiz eingetragen)* |
| **DL-076** | **Rise 360 abgelöst.** Lektionen selbstgebaut als Markdown + Frontmatter, Block-Renderer, kein iframe mehr · 12 Lektionen über 3 Phasen · Video: selbstgehostet MP4 · Aufwand ~2,5 PT · korrigiert DL-030. Ready Check von dieser Entscheidung nicht erfasst (offen) |
| **DL-032** | Markenauftritt teilt sich: Marketing = Kado-Submarke · Shell = eigenständig · *Korrekturnotiz durch DL-041* |
| **DL-039** | Home-Tab als vierter, gleichrangiger Tab · **Standard-Landing-Tab bei jedem Rückbesuch** · Element-Inventar · *Korrekturnotizen durch DL-045 und DL-052* |
| **DL-041** | Nav teilt sich nach Breakpoint: Desktop = volle Labels · Mobil = Burger · Kundenlogo-Slot · *Korrekturnotizen durch DL-044* |
| **DL-044** | **Einstellungen** als vierter Navigationsbereich · Zahnrad-Icon · 40px Gap · Inhalt: Code · weiteres Gerät · E-Mails · Peergruppe · *Korrekturnotiz durch DL-067 (auf Mobil ein Accordion)* |
| **DL-045** | Home-Hub-Inventar · Hero mit vier Zuständen · Webinar-Liste · Coach-Widget · *Prompt-Bereich entfällt ersatzlos (Korrekturnotizen durch DL-052 und DL-061)* |
| **DL-047** | Icons: Lucide · Konventionen: stroke an Token · 16/20/24px je nach Kontext |
| **DL-048** | **Ein hartes Datums-Gate:** nur Momentum · Impulsphase offen ab Einladung · Werkstatt fortschrittsbasiert |
| **DL-049** | Vor-Start-Zustand entfällt · erste Impuls-Lektion trägt Programmüberblick und Peergruppen-Erklärung |
| **DL-050** | Webinare: **empfohlen, nicht Pflicht** · keine Aufzeichnungen · Content muss ohne Webinare funktionieren |
| **DL-052** | **Aufgabenliste = Fristenliste**, keine Checkliste · Aufgabe erscheint, wenn der Kurs den Kontext erklärt hat · kein Rot |
| **DL-054** | Button-Property `ExternalIcon` für jeden Button, der die Shell verlässt · Lucide `external-link` 16px |
| **DL-060** | Wizard Schritt 2: **beobachtete Handlung** statt Zwangswahl · PDF-Download oder mailto: schaltet „Weiter" frei · Kaskade `Wizard 2 — Hilfe` als letzter Ausweg |
| **DL-061** | Home-Prompt-Bereich **entfällt ersatzlos** · kein aufgeschobener Zustand kann noch entstehen |
| **DL-062** | **Fehlerseite:** ein Frame, vier Zustände (B: pid ungültig · C: pid abgelaufen · E: Catalyst nicht erreichbar · F: keine pid — einziger mit Code-Feld) |
| **DL-067** | **Einstellungen — Mobile ist ein Accordion** (vier Kopfzeilen + Untertitel, Inhalt auf Tap) · Desktop unverändert · neue Komponente `Accordion — Einstellungskarte` (Zu/Offen) nötig · korrigiert DL-044 |

---

## Formulare, Content, Provider

| DL | Inhalt |
|---|---|
| **DL-027** | Fillout → **Zoho Forms** · *Korrekturnotiz durch DL-070 (native In-Shell-Inputs als Default; Zoho Forms nur noch für Ready Check + Peergruppe)* |
| **DL-034** | **Mistral AI** als AI-Coach-Provider |
| **DL-038** | Coaching-Booking-Flow: **Zoho Bookings** als alleinige Kalender-Autorität · eigener Bookings-Service pro `pid` · **server-side only** · *Korrekturnotiz durch DL-042* |
| **DL-070** | **Native In-Shell-Inputs** ersetzen Zoho Forms als Default für Momentum-Reflexionen + Veränderungswerkstatt · Zoho Forms: nur Ready Check + Peergruppen-E-Mail · korrigiert DL-027 |

---

## Design-System

| DL | Inhalt |
|---|---|
| **DL-041** | Nav-Komponenten, Slot-Geometrie · *Korrekturnotizen durch DL-044* |
| **DL-043** | **Eine braune Markenfamilie** (#B37357 = 500) · #3A5A54 = Success, **nur** semantisch · **kein Warning-Token** · Icons: Lucide, selbst gehostet |
| **DL-046** | Coach-Widget: **Copy eingefroren** (nicht optimieren) · `coachName` + `coachImageUrl` pro `pid` · Bild unter `*.k-a-d-o.com` |
| **DL-047** | Lucide-Icons: `fill = none` · Stroke an Token · 16/20/24px je nach Kontext |
| **DL-051** | Wizard-Copy: **„Auch wir nicht"** hat dieselbe Schutzklasse wie Coach-Widget-Copy — nicht weichspülen · *siehe auch DL-064 (war an drei von drei Stellen aufgeweicht)* |
| **DL-064** | Eingefrorene DL-042-Copy („Auch wir nicht") stand an **drei von drei Stellen aufgeweicht** im File (Standardtext, kein Ausrutscher) · Regel: **jede Zahl/Nummer/ID auslesen, nicht raten** |
| **DL-065** | **„Konto" im teilnehmerseitigen Text verboten** (habify30 hat keins) · Magic Link = Schlüssel, kein Login · Glossar-Eintrag · Sprachregel `kado-content-voice` |
| **DL-077** | **Figma-Split**: DS als publizierte Team-Library (`jO1gy…`), Screens im neuen File `habify30 Screens` (`3U4mfB…`), das konsumiert · Cleanup gegen KONV-figma · `— FRAMES —`-Referenzen historisch · Skill `habify30-figma` abgelöst |
| **DL-078** | **Sechs UX-Kriterien** verbindlich (Handlungsbezug · Weglass-Test · kein Erklärungsbedarf · eine Handlung/Seite · Mobile first · Kontext-Reinheit) |

**Wichtig:** Rot ist ausschließlich für Fehler reserviert. Eine Frist ist kein Fehler (DL-052).

---

## Produktkern, Scope, Philosophie

| DL | Inhalt |
|---|---|
| **DL-001** bis **DL-018** | Transfer statt Lernen · B2B · ein Verhalten zur Zeit · beobachtbare Verhaltensweisen · Experimente statt Verpflichtungen · leichtgewichtige Reflexion · Alltag als Umsetzungsumgebung · niedrige Komplexität |
| **DL-024** | Repository-Scope: Produktionsmaterial gehört dazu |
| **DL-025** | Scope-Grenze ist **severity-basiert**, nicht typ-basiert |
| **DL-079** | **habify-Skills im Repo** (`skills/<name>/SKILL.md`) als Quelle der Wahrheit · UI-Upload = Deployment · Familie decision-propagation + session-state + catalyst-probing abgelegt |
| **DL-080** | **DL-Log-Split**: ein File je Eintrag (`decisions/DL-NNN_<slug>.md`) + leichtes Frontmatter (status/supersedes) · Historie erhalten, kein Konsolidieren · Migration skript-gestützt + verifiziert |

---

## Ready Check

| DL | Inhalt |
|---|---|
| **DL-021** | Ready Check als ausschließendes Gate · *aufgehoben durch DL-023* |
| **DL-023** | **Kein Gate mehr.** Eigenständiges, registrierungsfreies Empfehlungswerkzeug ohne technische Verbindung zum Hauptprogramm |
| **DL-033** | Eigene Shell, unabhängig vom `pid`-Lebenszyklus des Hauptprogramms |

---

## Technische Plattform, Frontend-Hosting, Datenabfrage

| DL | Inhalt |
|---|---|
| **DL-068** | **Catalyst Slate** als Frontend-Host (ersetzt Web Client Hosting) · SPA-Routing HTTP 200 · Root-Base-Path · Static-Framework · mehrere Apps/Projekt · Cache `max-age=31536000` auf Shell-HTML (Hash-Assets nötig) · korrigiert DL-028 |
| **DL-069** | **Native ZCQL-Aggregation** trägt das Dashboard · GROUP BY / AVG / SUM / COUNT / korrelierte Subqueries bestätigt · `COUNT(DISTINCT)` ignoriert DISTINCT still (Falle) · kein freier JOIN · OLAP nicht verfügbar · Insert_Rows cappt bei 200 |
| **DL-081** | **State-/Speicher-Vertrag der Shell:** ein `localStorage`-Store `h30.state` (typisiert, `schemaVersion`) als Gerätewahrheit · Catalyst Data Store + Stratus als Backend · fixer Andockpunkt für das offene Capabilities-Objekt (OQ-028, additiv) · reservierte, aber offene Namespaces (`progress` gehört DL-076) |
| **DL-082** | **Build-Reconciliations** (erster Shell+Backend-Bau): CORS via **Authorized Domains** statt manueller Function-Header (korrigiert DL-029; Catalyst beantwortet OPTIONS am Gateway) · Ablauf über **eine `expiry_date`-Spalte** statt DL-031-Formel (verfeinert DL-058) · Ordner `Catalyst_Functions/` → `functions/` (CLI-Standard) · Spalten snake_case (`programm_name`/`contact_email`/`expiry_date`) |

**Vor dem Bau von:** Dashboard-Queries (DISTINCT-Falle beachten), Shell-Deploy-Pipeline (Hash-Asset-Namen), **jedem Screen mit Client-State (DL-081)**.

---

## AI-Coach

| DL | Inhalt |
|---|---|
| **DL-034** | **Mistral AI** als AI-Coach-Provider |
| **DL-071** | **Drei-Tier-Architektur** (Tier 1: kein AI · Tier 2: Bot ohne Gedächtnis · Tier 3: voller Coach mit user-carried session-state) · Hebel-Bundle H1/H2/H3/H5/H6 · H4/H7 nicht im Startpaket |
| **DL-072** | **User-carried session-state** — keine serverseitige Konversationspersistenz · Art.-9-Free-Text bleibt außerhalb Kado-Infrastruktur |
| **DL-073** | **Selbstgewählte Themenlabels** (uid-gebunden · grob · nicht KI-abgeleitet · Art. 9(2)(a) · separates Opt-in nach Wizard) · Opt-in-Formulierung offen (OQ-034) |
| **DL-074** | **Lösch-Log in Catalyst Stratus** (EU-Bucket · append-only · getrennt vom Data Store · überlebt Zoho-Restore) · manuelle Stratus-Initialisierung per Konsole |
| **DL-075** | **No-Profiling-Guardrail** · keine RisikoProfile aus kombinierten Signalen (Art. 22) · etabliert Canon C-020 · Mistral-GCP-EU-Residency offen (OQ-033) |
| **Canon C-020** | **Kein Profiling.** Immutable principle. |

**Vor dem Bau von:** allem, was mit dem AI-Coach zu tun hat. DL-073 (Themenlabels und Art.-9-Opt-in), DL-072 (kein serverseitiges Speichern von Konversationsinhalt) und Canon C-020 (kein Profiling) sind die zentralen Constraints.

---

## Hinfällig / überholt

| DL | Status |
|---|---|
| **DL-021** | Aufgehoben durch DL-023 |
| **DL-022** | Aufgehoben durch DL-030 (SCORM → Web Export) |
| **DL-028 (Hosting OVHcloud/Web Client)** | Frontend-Host zu Catalyst Slate gewechselt — Korrekturnotiz durch DL-068 eingetragen |
| **DL-030 (Rise/iframe-Mechanik)** | Ersetzt durch DL-076 (Markdown-Lektionen, kein iframe) — Korrekturnotiz eingetragen. Release-Gating-Prinzip selbst bleibt in Kraft |
| **DL-031 (Hard-Lock-Fall)** | „URL pid absent, cache empty → Hard Lock mit manueller pid-Eingabe" — **ersetzt durch Fehlerseite Zustand F** (DL-062). Korrekturnotiz in DL-031 eingetragen. |

---

# Wie man diesen Index benutzt

1. **Thema identifizieren** (z. B. „Peergruppen-Anmeldung bauen")
2. **Hier nachsehen**, welche DL-Nummern es berühren → DL-035, DL-036, DL-037
3. **Gezielt lesen**: die betroffene Datei öffnen, `decisions/DL-036_*.md` (ein File je Eintrag seit DL-080).
4. **Erst dann bauen.**

Die Einträge liegen einzeln unter [`decisions/`](decisions/); die frühere
Am-Stück-Trunkierungswarnung entfällt (kein Monolith mehr). `09_Decision_Log.md` ist nur noch
Einstieg + stehende Abschnitte.
