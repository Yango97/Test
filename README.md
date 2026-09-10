# Unsere Bucket List

Eine gemeinsame Liste für Dinge, die zwei Personen zusammen unternehmen wollen. Läuft als reine Webseite über GitHub Pages, keine Anmeldung nötig — für die gemeinsame Speicherung wird ein kostenloses Firebase-Projekt (Firestore) angebunden.

## Einmalige Einrichtung

### 1. Firebase-Projekt anlegen
1. Auf [console.firebase.google.com](https://console.firebase.google.com) mit einem Google-Konto anmelden.
2. "Projekt hinzufügen" -> Namen vergeben (z. B. `abenteuerliste`) -> Projekt erstellen (Google Analytics kann deaktiviert bleiben).

### 2. Firestore-Datenbank aktivieren
1. Im Projekt links auf **Build -> Firestore Database** -> "Datenbank erstellen".
2. Standort wählen, Modus **"Testmodus"** (offene Regeln) für den Start.
3. Nach dem Anlegen unter **Regeln** folgendes eintragen und veröffentlichen:
   ```
   rules_version = '2';
   service cloud.firestore {
     match /databases/{database}/documents {
       match /items/{itemId} {
         allow read, write: if true;
       }
     }
   }
   ```
   Das erlaubt jedem mit dem Seiten-Link, die Liste zu lesen und zu bearbeiten — ohne Login. Passend für eine private, nicht öffentlich beworbene Liste.

### 3. Web-App registrieren & Konfiguration eintragen
1. Projektübersicht -> Zahnrad -> **Projekteinstellungen** -> unten bei "Meine Apps" auf das Web-Symbol `</>` klicken -> App registrieren (Firebase Hosting nicht nötig).
2. Die angezeigten Werte (`apiKey`, `authDomain`, `projectId`, …) in die Datei [`firebase-config.js`](./firebase-config.js) an Stelle der Platzhalter eintragen.
3. Änderungen committen und pushen.

### 4. GitHub Pages aktivieren
1. Im Repository: **Settings -> Pages**.
2. Unter "Build and deployment" -> Source: **Deploy from a branch**.
3. Branch auswählen (aktuell `claude/shared-activity-list-xsh0ma`, oder nach dem Mergen `main`), Ordner `/ (root)` -> Speichern.
4. Nach ein bis zwei Minuten ist die Seite unter der von GitHub angezeigten URL erreichbar (Format `https://<konto>.github.io/<repo>/`).

## Nutzung

Beide Personen öffnen den GitHub-Pages-Link im Browser, tragen einmalig ihren Namen ein (nur lokal auf dem jeweiligen Gerät gespeichert) und sehen danach dieselbe, live synchronisierte Liste.

## Hinweis zur Sicherheit

Die offenen Firestore-Regeln bedeuten: Wer den Link kennt, kann die Liste lesen und ändern — es gibt keinen Passwortschutz. Für eine private Bucket-Liste ist das ein bewusster, üblicher Kompromiss. Wer mehr Schutz möchte, kann später Firebase Authentication ergänzen.
