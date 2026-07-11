# VindJeFysio Netwerk — armklachten.net (Arm Netwerk)

Deze repo is één "spoke" in een hub-and-spoke netwerk van gespecialiseerde fysiotherapie-subsites. De hub is vindjefysio.net; spokes zijn o.a. rugnek, enkelvoet, beenklachten, kansrijkopgroeien, mentaalgezond, chronischezorg en deze (armklachten).

## Architectuur
- Statische HTML/CSS/vanilla JS op GitHub Pages, custom domein via CNAME.
- Gedeelde Supabase-backend, project islujznszevdynguhjdc, met anon key in de frontend.
- Gedeelde tabellen: therapeuten, praktijken, therapeut_subcategorieen, therapeut_praktijken, subcategorieen, categorieen.
- Elke spoke deelt dezelfde structuur en bestanden: index.html, mijn-profiel.html, therapeuten.html, protocollen.html (gegenereerd), sync_protocollen.py, .github/workflows/sync-protocollen.yml, protocollen-config.json. Let op: deze repo noemt het aanmeldbestand `aanmelden-therapeut.html` (niet `therapeut-aanmelden.html` zoals de meeste andere spokes) en heeft een typo-bestand `pricavy.html` (bedoeld als privacy.html) — check bij links welk bestand daadwerkelijk bestaat.

## Belangrijke conventies
- Het therapeut-aanmeldformulier linkt ALTIJD relatief/lokaal binnen de eigen subsite (nooit naar vindjefysio.net).
- Praktijk/locatie-aanmelden loopt WEL centraal via vindjefysio.net/aanmelden.html?via=<domein>.
- Mails lopen via info@vindjefysio.net.
- Therapeut-registratie zet aangemeld_via op het eigen subsite-domein en actief=false (wacht op goedkeuring).
- Deze site: palet primair teal #17A398 (met teal-dark #127F76), navy #213A4C. index.html bevat daarnaast een gedeelde utility-set groen #6ABF4B, blauw #2E7DD1, oranje #F5A623 (niet het hoofdpalet van déze site, maar gedeelde CSS-variabelen).
- Structuur = drie DOMEINEN (elk een eigen kleur, zie `DOMEINEN`-object in index.html) + domein-overstijgende thema's:
  - Schouder en bovenarm (kleur #17A398) — schouderpijn/frozen shoulder, impingement/rotator cuff
  - Elleboog en onderarm (kleur #E8952E) — tenniselleboog & golferselleboog
  - Pols en hand (kleur #2E86C1) — carpaal tunnel & tintelingen, pols-/duim-/handklachten
  - Thema's (kleur #5B7285, combineren met elk domein): revalidatie na operatie, voorbereiding op operatie, sport/bewegen/overbelasting
- ⚠️ Er staat een placeholder in index.html: subcategorie-id 999 = 'Borst- en ribklachten' bestaat nog niet in Supabase. Bij het aanmaken van die subcategorie moet 999 overal vervangen worden door het echte id (ook in het aanmeldformulier).

## Sync-pipeline
- sync_protocollen.py haalt protocollen op uit publiek gedeelde Google Docs (export-link, geen API-key), zet markdown om naar HTML, genereert protocollen/<id>-makkelijk.html (patiënt) en -complex.html (therapeut) + protocollen.html + sitemap.xml.
- De workflow (.github/workflows/sync-protocollen.yml) git-add regel moet zijn: git add protocollen/ protocollen.html sitemap.xml (op één regel).
- Google Docs moeten op "iedereen met de link kan bekijken" staan.
