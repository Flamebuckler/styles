# Styles

Dieses Repository enthält Basisstyles für mehrere Webprojekte. Die Dateien sind modular aufgebaut (`tokens`, `base`, `components`), damit einzelne Projekte nur die benötigten Teile importieren können.

## Struktur

```
styles/
├─ src/
│  ├─ tokens/
│  │  ├─ _colors.scss      ← CSS-Variablen für verschiedene Farbschemata (Farben, Akkzente)
│  │  └─ _spacing.scss     ← einheitliche Abstände/Spacing-Werte
│  ├─ base/
│  │  ├─ _reset.scss       ← Reset, Grundtypen, Box-Sizing
│  │  ├─ _typography.scss  ← Schriftfamilien, Überschriften, Textelemente
│  │  └─ _container.scss   ← zentrierte Breite mit Padding
│  ├─ components/
│  │  ├─ _layout.scss      ← Header, Main, Footer, Header-Utilities
│  │  ├─ _tool.scss        ← Link-Kacheln
│  │  ├─ _cards.scss       ← allgemeine Kacheln und Ergebnisanzeigen
│  │  ├─ _forms.scss       ← Formulare, Search‑Input, Buttons
│  │  └─ _tables.scss      ← Tabellen für Ergebnislisten
│  ├─ utilities/
│  │  └─ _helpers.scss     ← Hilfsklassen (.visually-hidden, .hidden, Helferklassen)
│  └─ main.scss            ← zentraler Import aller Module
```

## Nutzung in einem Projekt

Dieses Repo kann als normaler npm‑/yarn‑Package installiert werden. Es besteht auch aus einer reinen
SCSS‑Bibliothek; die erzeugte CSS‑Datei liegt nach dem Build unter `dist/styles.css`.

### Schritt-für-Schritt Einbindung (öffentliche Repository)

1. **Abhängigkeit hinzufügen**

   ```bash
   cd /pfad/zum/webprojekt
   npm install git+https://github.com/flamebuckler/styles.git
   # oder mit yarn:
   # yarn add git+https://github.com/flamebuckler/styles.git
   ```

   Der Code landet in `node_modules/styles`.

2. **SCSS importieren**
   - In der zentralen SCSS‑Datei (`main.scss`, `app.scss` etc.):
     ```scss
     @use 'styles/src/main' as *; // importiert alle Module
     // Du kannst alternativ auch nur Teilmodule laden:
     // @use "styles/src/components/forms" as *;
     // @use "styles/src/tokens/colors" as *;
     ```
   - Falls deine Toolchain noch `@import` nutzt, funktioniert auch
     `@import "styles/src/main";`.

3. **Build ausführen**
   Lass deinen SCSS‑Compiler (Webpack, Vite, Sass‑CLI, Jekyll etc.) laufen. Die
   Styles werden automatisch zusammengestelt.

4. **Optional: Fertiges CSS verwenden**
   Wenn du nicht kompilierst, kannst du die vorgefertigte Datei einsetzen:
   - `node_modules/styles/dist/styles.css` nach `assets/` kopieren, oder
   - das Artefakt aus dem GitHub‑Action‑Workflow herunterladen.
     Binde sie in HTML via `<link rel="stylesheet" href="/pfad/styles.css">`.

5. **Projektspezifische Anpassungen**
   Schreibe eigene Regeln _nach_ dem Import, um zentrale Styles zu überschreiben.

### Alternative: Git-Submodule

1. Submodule hinzufügen:

   ```bash
   git submodule add https://github.com/flamebuckler/styles.git styles
   git submodule update --init --recursive
   ```

2. In SCSS exportieren:

   ```scss
   @use '../styles/src/main' as *; // Pfad ggf. anpassen
   ```

3. Neuerungen übernehmen:
   ```bash
   cd styles && git pull
   git add styles && git commit -m "update styles submodule"
   ```

### Beispiel `package.json` im styles‑Repo

```json
{
	"name": "styles",
	"version": "0.1.0",
	"description": "Gemeinsame Basisstyles für die Web‑Projekte",
	"main": "dist/styles.css",
	"scripts": {
		"build": "sass src/main.scss dist/styles.css --no-source-map --style=compressed"
	},
	"devDependencies": {
		"sass": "^1.97.3"
	}
}
```

Nach dem Build können andere Repos entweder das Paket installieren oder einfach die fertige CSS-Datei
per CDN/Link einbinden.

Das Repository enthält eine Basis‑Stylelint‑Konfiguration (`.stylelintrc.json`), die Du in den
Projekten wiederverwenden kannst, um einheitliche CSS‑Regeln (Einrückung, Farbcodierung etc.)
durchzusetzen.

## Namenskonventionen & Tokens

- **CSS‑Custom‑Properties** erhalten einheitliche Präfixe:

> Hinweis: Im Quellcode nutzen wir `@use 'path' as *;` statt des veralteten `@import`. Das passt
> zur aktuellen Sass‑Richtlinie und vermeidet Warnungen beim Build.

- Farben: `--color-…` (z. B. `--color-dark-background`, `--color-flame-muted`)
- Abstände: `--space-…` (z. B. `--space-md`, `--space-2xs` für 12 px); verwende diese Tokens im gesamten Projekt anstelle von
  harten Pixelwerten – Beispiel: `padding: var(--space-lg);` statt `padding: 24px;`)
- Weitere Kategorien (z‑Index, Radius etc.) folgen demselben Schema.
- Komponentennamen orientieren sich an einfachen, sprechenden Klassen. Bei größeren Projekten
  empfiehlt sich BEM oder ein ähnliches System, um Überschneidungen zu vermeiden.
- Utility‑Klassen (z. B. `.text-center`, `.mt-2`) werden im `utilities/_helpers.scss` gesammelt.

1. Paket installieren (lokal oder per GitHub‑URL):

   ```bash
   # aus dem Repository installieren – keine Veröffentlichung in einer Registry nötig
   npm install git+https://github.com/flamebuckler/styles.git
   ```

2. In deinem SCSS/Buildsystem einbinden:

   ```scss
   // modern: via @use, damit keine Sass‑Deprecation‑Warnungen auftreten
   @use 'styles/src/main' as *; // oder spezifische Module

   // falls dein Projekt noch @import verwendet, geht auch
   // @import 'styles/src/main';
   ```

3. Bei Bedarf Komponenten ergänzen oder eigene Regeln hinzufügen.

## Extrahieren bestehender Styles

Der oben gezeigte Katalog stammt aus zwei konkreten Projekten. Beim zweiten Stylesheet etwa wurden durch die Aufteilung folgende Modulzuordnungen erzeugt:

- **Farben und Tokens** → `tokens/_colors.scss` (alle Variablen, z. B. `--color-dark-background`, `--color-flame-card`, `--color-flame-text` usw.)
- **Typografie & Grundregeln** → `base/_typography.scss` (Schrift‑Stacks, Überschriften, `.tagline`, `.hint`)
- **Layout/Container** → `base/_container.scss` plus Erweiterungen von Header/Footer in `components/_layout.scss`
- **Cards/Formulare/Tabellen** → neue Komponenten (`_cards.scss`, `_forms.scss`, `_tables.scss`)
- **Utilities** → `.visually-hidden` und `.hidden` im `utilities/_helpers.scss`

Prozess:

- Öffne die CSS der Website und kopiere jeweils eine Regeln in das passende Modul – bereinige Duplikate.
- Teste das Ergebnis visuell gegen die ursprüngliche Seite.
- Inhalte, die wirklich nur in einem Projekt gebraucht werden, dürfen im Projekt‑Repo bleiben; dann werden sie **nach** dem Import der zentralen Styles geladen.

Dies erleichtert zukünftige Wartung und macht es möglich, beide Farbpaletten nebeneinander zu verwenden oder eine gemein‑same Basis zu starten und bei Bedarf pro Projekt abzuweichen.

## Automatisierung

Beim Push in das Repo generiert eine GitHub‑Action automatisch die kompilierte
`dist/styles.css` und hängt sie als Artefakt an (siehe `.github/workflows/build.yml`).
So steht stets eine frische CSS‑Version zur Verfügung, ohne dass Du
lokal kompilieren musst.
