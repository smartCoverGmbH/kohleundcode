# Kohle & Code — Website

Statische Website, fertig zum Hosten über GitHub Pages. Keine Build-Schritte, keine
Abhängigkeiten, kein Node, kein Framework. Jede Seite ist eine einzelne HTML-Datei mit
eingebettetem CSS und JavaScript.

## Dateien

| Datei | Zweck |
|---|---|
| `index.html` | Startseite: Arbeiten, Live-Demo, Prinzipien, Ablauf, Pakete, Wartungspaket, FAQ, Kontaktformular |
| `impressum.html` | Impressum nach § 5 DDG |
| `datenschutz.html` | Datenschutzerklärung nach Art. 12 ff. DSGVO |
| `robots.txt` | Suchmaschinen-Freigabe, verweist auf die Sitemap |
| `sitemap.xml` | Seitenverzeichnis für Suchmaschinen |
| `favicon.ico` · `favicon.svg` · `icon-48/96/144/192/512.png` | Logo für Browser-Tab, Lesezeichen und Google-Suchergebnis |
| `apple-touch-icon.png` | Logo, wenn jemand die Seite auf den iPhone-Homescreen legt |
| `site.webmanifest` | Icon-Verzeichnis für Android und installierbare Web-Apps |
| `og-bild.png` | Vorschaubild beim Teilen per WhatsApp, LinkedIn, Slack |
| `CNAME` | Sagt GitHub Pages, dass die Seite unter `kohleundcode.de` läuft |
| `.nojekyll` | Schaltet die Jekyll-Verarbeitung auf GitHub Pages ab |

## Auf GitHub Pages veröffentlichen

Zielrepository: **github.com/smartCoverGmbH/kohleundcode**, Zieladresse **https://kohleundcode.de**.
`CNAME`, `robots.txt` und `sitemap.xml` sind bereits darauf eingestellt — es ist nichts auszufüllen.

1. Alle Dateien aus diesem Ordner in das Repository hochladen. Der Web-Upload reicht:
   **Add file → Upload files**, dann alle Dateien hineinziehen und committen.
   Wichtig: `index.html` muss in der obersten Ebene liegen, nicht in einem Unterordner.
2. Im Repository auf **Settings → Pages** gehen.
3. Unter *Build and deployment* bei *Source* **Deploy from a branch** wählen,
   als Branch `main` und als Ordner `/ (root)`. Speichern.
4. Nach ein bis zwei Minuten ist die Seite erreichbar unter
   **`https://smartcovergmbh.github.io/kohleundcode/`**
   (der Host wird von GitHub kleingeschrieben, auch wenn die Organisation `smartCoverGmbH` heißt).
   Diese Adresse ist die Zwischenstation — sobald die Domain steht, leitet GitHub sie
   automatisch auf `kohleundcode.de` um.

Das Repository muss **öffentlich** sein, sonst blockt GitHub Pages im kostenlosen Tarif die
Veröffentlichung. Die Dateien enthalten nichts Vertrauliches — Impressum und Kontaktdaten sind
ohnehin für die Öffentlichkeit bestimmt.

Die Datei `.nojekyll` sorgt dafür, dass GitHub die Dateien unverändert ausliefert. Sie ist leer
und beginnt mit einem Punkt — beim Upload über den Browser bitte darauf achten, dass sie mitkommt.

Alle internen Links sind relativ gesetzt. Die Seite funktioniert deshalb sowohl im Unterpfad
`/kohleundcode/` als auch unter der eigenen Domain, ohne dass etwas geändert werden muss.

## kohleundcode.de bei STRATO verbinden

Ziel: `kohleundcode.de` ist die Hauptadresse, `www.kohleundcode.de` leitet dorthin um.
Die Reihenfolge ist wichtig — erst DNS, dann GitHub.

### Schritt 1 — DNS bei STRATO setzen

Im STRATO Kundenlogin: **Domains → Domainverwaltung**, beim Eintrag `kohleundcode.de` auf das
Zahnrad klicken, dann in den Bereich **DNS-Verwaltung**.

**A-Record** (für `kohleundcode.de` ohne www): von *STRATO Standard* auf **Eigene IP-Adresse**
umstellen und diese vier Adressen von GitHub eintragen:

```
185.199.108.153
185.199.109.153
185.199.110.153
185.199.111.153
```

Lässt die STRATO-Maske nur eine einzige IP zu, nimm `185.199.108.153`. Das funktioniert, kostet
aber die Ausfallredundanz der übrigen drei Server — im Zweifel bei STRATO nachfragen, ob sich
mehrere A-Records anlegen lassen.

**AAAA-Record** (optional, für IPv6 — empfehlenswert, viele Anschlüsse sind heute IPv6-first):

```
2606:50c0:8000::153
2606:50c0:8001::153
2606:50c0:8002::153
2606:50c0:8003::153
```

**CNAME-Record** für die Subdomain `www`: Ziel ist `smartcovergmbh.github.io` (mit Punkt am Ende,
falls die Maske einen verlangt). Bei STRATO sind CNAME-Einträge nur auf Subdomain-Ebene möglich —
für `kohleundcode.de` selbst geht deshalb nur der A-Record oben, das ist so korrekt.

Achtung: Sobald bei einem Host ein CNAME aktiv ist, deaktiviert STRATO dort alle anderen Einträge.
Das betrifft nur `www`, nicht die Hauptdomain.

### Schritt 2 — In GitHub eintragen

Die Datei `CNAME` in diesem Ordner enthält bereits `kohleundcode.de`. Nach dem Upload erkennt
GitHub die Domain automatisch. Zur Kontrolle: **Settings → Pages → Custom domain** sollte
`kohleundcode.de` anzeigen und darunter ein grünes *DNS check successful*.

Steht dort eine Fehlermeldung, ist die DNS-Änderung noch nicht durchgelaufen — einfach später
noch einmal auf **Check again** klicken.

### Schritt 3 — HTTPS aktivieren

Sobald der DNS-Check grün ist, stellt GitHub automatisch ein Let's-Encrypt-Zertifikat aus. Danach
im selben Menü **Enforce HTTPS** ankreuzen. Das Häkchen lässt sich erst setzen, wenn das
Zertifikat fertig ist.

### Wie lange dauert das?

DNS-Änderungen bei STRATO sind meist nach 15 bis 60 Minuten aktiv, im Extremfall dauert es bis zu
24 Stunden. Das Zertifikat kommt danach innerhalb weniger Minuten. Prüfen lässt sich der Stand mit
`nslookup kohleundcode.de` — solange dort noch eine STRATO-IP steht, ist die Änderung nicht durch.

### Wenn die Domain vorher woanders hing

Falls `kohleundcode.de` bei STRATO bisher auf ein Webhosting-Paket oder eine Parkseite zeigte,
muss diese Zuordnung aufgehoben sein, sonst überschreibt STRATO den A-Record wieder. E-Mail-Adressen
unter der Domain sind davon **nicht** betroffen — die hängen an den MX-Einträgen, und die bleiben
unangetastet.

## Das Kontaktformular

Das Formular sendet **nichts** an einen Server. Beim Absenden baut ein kleines Skript aus den
Eingaben eine fertige E-Mail und öffnet damit das E-Mail-Programm der Besucherin oder des
Besuchers. Empfängeradresse ist `kohleundcode@smartcover.gmbh`.

Falls kein E-Mail-Programm eingerichtet ist, erscheint nach kurzer Zeit ein Kasten mit dem
fertigen Text zum Kopieren. Damit funktioniert das Formular auch dann, wenn `mailto:` ins Leere läuft.

Das hat zwei Konsequenzen, die man kennen sollte: Es entsteht keine Datenverarbeitung auf unserer
Seite, also braucht es weder einen Auftragsverarbeitungsvertrag noch einen Formular-Absatz in der
Datenschutzerklärung, der über die jetzige Formulierung hinausgeht. Dafür gehen erfahrungsgemäß
einige Anfragen verloren, weil nicht jeder ein Mailprogramm eingerichtet hat. Wenn das später stört,
lässt sich ein Dienst wie Formspree oder Web3Forms nachrüsten — dann kommen ein AV-Vertrag und ein
zusätzlicher Absatz in der Datenschutzerklärung dazu.

### Empfängeradresse ändern

In `index.html` nach dieser Zeile suchen:

```js
var MAIL = 'kohleundcode' + '@' + 'smartcover.gmbh';
```

Die Adresse ist absichtlich zusammengesetzt, damit einfache Adress-Sammler sie nicht direkt
im Quelltext finden. Wer sie ändert, sollte auch die drei sichtbaren Vorkommen im Kontaktbereich,
im Impressum und in der Datenschutzerklärung anpassen.

## Was noch zu tun ist

- [ ] Die drei Beispiel-Arbeiten auf der Startseite sind Platzhalter-Grafiken. Sobald echte
      Projekte live sind, gehören dort Screenshots und die echten Domains hinein.
- [ ] Die Preise stehen fest im HTML (`1.490 €`, `2.900 €`, `ab 4.900 €`, `49 €/Monat`).
      Bei Änderungen an drei Stellen anpassen: Paketkarten, Wartungsband, FAQ.
- [ ] Nach dem Livegang die Seite bei der Google Search Console anmelden und dort die
      Sitemap `https://kohleundcode.de/sitemap.xml` einreichen.
- [ ] Impressum und Datenschutzerklärung vor dem Livegang anwaltlich durchsehen lassen.
- [ ] Wenn später Analytics, Karten, Schriften oder eingebettete Videos dazukommen, stimmen die
      Aussagen „keine Cookies, keine fremden Server“ nicht mehr. Dann müssen Datenschutzerklärung,
      Footer-Zeile und die Prinzipien-Kachel „Technisch datensparsam“ angepasst und ein
      Einwilligungsbanner ergänzt werden.

## Logo in Tab und Google-Suche

Das Logo liegt als echte Bilddateien im Ordner, nicht mehr als eingebetteter Data-URI. Das ist
Voraussetzung dafür, dass Google es überhaupt einlesen kann — Data-URIs crawlt der Bot nicht.

- **Browser-Tab und Lesezeichen:** `favicon.ico` (16/32/48 px) und `favicon.svg` (skaliert
  verlustfrei auf Retina-Displays).
- **Google-Suchergebnis:** `icon-48.png` bis `icon-512.png`. Google verlangt ein Quadrat, dessen
  Kantenlänge ein Vielfaches von 48 px ist — deshalb diese Größen und nicht die alten 64 px.
- **iPhone-Homescreen:** `apple-touch-icon.png`, 180 px, mit dunklem Hintergrund statt Transparenz,
  weil iOS transparente Icons sonst schwarz auf schwarz zeigt.
- **Teilen-Vorschau:** `og-bild.png`, 1200 × 630 px, mit Wortmarke, Claim und Telefonnummer.

Alle drei HTML-Seiten verweisen relativ auf diese Dateien. Sie funktionieren deshalb sowohl unter
`kohleundcode.de` als auch unter der github.io-Zwischenadresse.

**Zum Zeitverhalten bei Google:** Das Icon erscheint nicht sofort. Google übernimmt es erst beim
nächsten Crawl der Startseite, das dauert je nach Seite Tage bis einige Wochen. Beschleunigen lässt
es sich, indem die Domain in der Google Search Console angemeldet und die Startseite dort einmal
manuell zur Indexierung eingereicht wird. Bedingung ist außerdem, dass `robots.txt` die Icons nicht
blockiert — tut sie hier nicht.

Wenn das Logo später einmal geändert wird: `favicon.svg` anpassen und die PNG-Größen daraus neu
exportieren, damit alle Varianten zusammenpassen.

## Technisches

- Keine externen Requests: keine Google Fonts, keine CDNs, kein Tracking. Logo und Icons liegen
  als eigene Dateien im Ordner, das Nav-Logo zusätzlich als Inline-SVG.
- Kein `localStorage`, keine Cookies.
- Animationen respektieren `prefers-reduced-motion`.
- Getestet auf Desktop und Mobil (390 px), ohne horizontales Scrollen und ohne Konsolenfehler.
