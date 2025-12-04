# Kutatási jegyzőkönyv (szintetikus)

## Projekt

**TermAPI** – billentyűzetközpontú TUI alkalmazás API-k felfedezésére, tesztelésére és hibakeresésére.

## Kutatás célja

A kutatás célja annak feltárása volt, hogy fejlesztők és informatikus hallgatók hogyan tesztelnek API-kat fejlesztés közben, milyen eszközöket használnak, hol akadnak el, és milyen felületi megoldások segítenék őket gyorsabb, biztonságosabb és érthetőbb munkafolyamatban.

Kiemelt kérdések:

- Milyen gyakran váltanak a felhasználók dokumentáció, API kliens, terminál, logok és environment változók között?
- Milyen hibák fordulnak elő leggyakrabban API teszteléskor?
- Mennyire zavaró a GUI-alapú kliensek kattintásigénye ismételt requesteknél?
- Hasznos-e egy külön hibakeresési nézet, amely nem csak státuszkódot mutat, hanem request contextet is?
- Mennyire fontos az aktív environment, auth mód és secret értékek látható, de biztonságos kezelése?

## Módszertan

### Kérdőív

7 válaszadó vett részt a kérdőíves kutatásban:

- 5 fejlesztő
- 2 informatikus hallgató

A kérdőív API tesztelési szokásokra, használt eszközökre, tipikus hibákra és workflow-problémákra kérdezett rá.

### Rövid interjúk

3 résztvevővel 15–20 perces félig strukturált interjú készült. A beszélgetések fókusza:

- endpoint keresés és böngészés
- request összeállítása
- environment váltás
- auth és token hibák
- response értelmezése
- request mentése, exportálása vagy megosztása

### Eszköz-összehasonlítás

4 meglévő megoldás használati mintái kerültek összehasonlításra:

- Postman
- Insomnia
- cURL / HTTPie
- Swagger UI

## Kutatási összefoglaló

Fő megállapítás: a felhasználók nem csak requestet akarnak küldeni. Gyorsan érteni akarják, miért sikeres vagy sikertelen egy request, és milyen contextben futott.

Legfontosabb pain pointok:

| Megfigyelés                                                        | Tervezési következmény                                                           |
| ------------------------------------------------------------------ | -------------------------------------------------------------------------------- |
| 6/7 válaszadó legalább két eszköz között vált API teszteléskor     | endpoint, request és response egy felületen jelenjen meg                         |
| 5/7 válaszadó hibázott már environment vagy token miatt            | aktív environment mindig látható legyen, secret értékek maszkolva jelenjenek meg |
| 4/7 válaszadó lassúnak érzi a GUI klienseket ismételt requestekhez | billentyűzetközpontú flow és command palette szükséges                           |
| Interjúkban visszatérő gond volt a 401/403 hibák okának feltárása  | Error Debugging nézet kell request contexttel és lehetséges okokkal              |

---

# Résztvevői profilok és kutatási jegyzetek

## 1. Generált Résztvevő G-01

### Résztvevői profil

- **Kor:** 24
- **Munkakör:** junior backend fejlesztő
- **Tapasztalat:** 1,5 év Node.js és Python backend fejlesztés, REST API-k implementálása és manuális tesztelése
- **Munkahelyi környezet:** kis termékfejlesztő csapat, ahol gyakran dolgozik lokális és staging API-kon
- **Technikai rutin:** sokat használ terminált, de API teszteléshez még gyakran Postmant nyit meg
- **Tesztelési helyzet:** új endpointot fejleszt, lokálisan működik, majd stagingen 401 vagy 500 hibát kap, és nem tudja rögtön eldönteni, hogy kódhiba, tokenhiba vagy rossz environment okozza

### Kutatási jegyzet

**Megfigyelés:** G-01 fejlesztés közben három-négy ablak között váltott: editor, terminál, Postman és lognézet. Endpoint tesztelésnél gyorsan összeállította a requestet, de stagingre váltáskor nem vette észre, hogy régi token maradt beállítva.

**Idézet:** „A státuszkódot látom, de utána mindig nekem kell kitalálni, hogy rossz-e a token, rossz-e az URL, vagy a backend halt meg.”

**Pain pointok:**

- rossz environment használata
- hiányzó vagy lejárt bearer token
- 401/403 hibák lassú diagnosztizálása
- túl sok kontextusváltás fejlesztés közben

**TermAPI-hoz kapcsolódó igények:**

- felső sávban állandó `Env` és `Auth` jelzés
- Error Debugging nézet, amely lehetséges okokat listáz
- request context panel tokenállapottal és aktív environmenttel
- gyors retry billentyűvel

**Tervezési következtetés:** G-01 számára a TermAPI legfontosabb értéke nem önmagában a request futtatása, hanem a hibakeresési context. A `401 Unauthorized` nézetben megjelenő „token missing”, „token expired” és „wrong credentials” típusú javaslatok csökkentik a bizonytalanságot.

---

## 2. Generált Résztvevő G-02

### Résztvevői profil

- **Kor:** 29
- **Munkakör:** full-stack fejlesztő
- **Tapasztalat:** 5 év React, TypeScript és backend integrációs munka; gyakran ellenőriz API response struktúrákat frontend fejlesztés előtt
- **Munkahelyi környezet:** közepes méretű SaaS csapat, staging és production API-k összehasonlítása rendszeres feladat
- **Technikai rutin:** magabiztos Postman- és cURL-használó, de zavarja a GUI kliensek kattintásigénye
- **Tesztelési helyzet:** frontend komponens építése előtt összehasonlítja, hogy stagingen és productionben azonos-e a response struktúra

### Kutatási jegyzet

**Megfigyelés:** G-02 gyorsan értette az endpoint browser szerkezetét. A response viewerben fontosnak tartotta a status, time, size és JSON body egyszerre látható megjelenítését. Külön kiemelte, hogy production response ellenőrzésnél nem szeretné, ha secret értékek véletlenül látszanának screen share közben.

**Idézet:** „Nem egy újabb nagy GUI kell, hanem valami, amiben gyorsan látom, mit adott vissza az API, és tudok diffelni két környezet között.”

**Pain pointok:**

- Postmanben sok kattintás egyszerű ismételt requestekhez
- history nehezen kereshető, ha sok hasonló request van
- secret adatok láthatósága képernyőmegosztásnál
- staging és production response összehasonlítása manuális

**TermAPI-hoz kapcsolódó igények:**

- billentyűzetes endpoint keresés
- response viewer status/time/size összefoglalóval
- `Diff` funkció staging és production response összehasonlításához
- secret értékek alapértelmezett maszkolása
- copy és export shortcutok

**Tervezési következtetés:** G-02 esetében a gyorsaság és biztonság együtt fontos. A TermAPI-nak meg kell őriznie a terminálos workflow sebességét, miközben strukturált response megjelenítést és secret maszkolást ad.

---

## 3. Generált Résztvevő G-03

### Résztvevői profil

- **Kor:** 21
- **Munkakör:** informatikus hallgató, gyakornok backend fejlesztő
- **Tapasztalat:** egyetemi projektekben REST API-k, alap cURL és Swagger UI használat; ipari API tesztelési tapasztalat 3 hónap
- **Munkahelyi környezet:** gyakornoki feladatoknál meglévő OpenAPI dokumentáció alapján próbál endpointokat tesztelni
- **Technikai rutin:** terminált használ, de komplex cURL parancsoknál könnyen hibázik; GUI eszközökben jobban eligazodik, ha látja az űrlapokat
- **Tesztelési helyzet:** OpenAPI specifikációból próbál endpointot keresni, majd path paramot, query paramot és headert kitölteni

### Kutatási jegyzet

**Megfigyelés:** G-03 Swagger UI-ban könnyen megtalálta az endpointokat, de terminálos cURL parancs összeállításánál elrontott egy headert és egy path paramot. A TermAPI lo-fi prototípusában az endpoint lista és request editor osztott panelje segített neki megérteni, hol kell adatot megadni.

**Idézet:** „A cURL gyors, de ha három header meg body is van, akkor inkább valami űrlaposabb nézet kellene.”

**Pain pointok:**

- cURL parancsok szintaktikai hibái
- kezdőként nehéz fejben tartani minden szükséges paramétert
- shortcutok nem egyértelműek első használatkor
- auth hibák okai nehezen érthetők

**TermAPI-hoz kapcsolódó igények:**

- OpenAPI import után böngészhető endpoint fa
- strukturált request builder path params, query params, headers, auth és body mezőkkel
- állandó shortcut bar minden képernyő alján
- Help nézet `?` billentyűvel
- szöveges státusz badge-ek szín mellett

**Tervezési következtetés:** G-03 számára a TUI tanulhatósága kritikus. A billentyűzetes gyorsaság önmagában nem elég; a shortcut bar, panelcímek és strukturált mezők segítik, hogy kezdő felhasználóként is biztonságosan tudjon requestet összeállítani.

---

## 4. Generált Résztvevő G-04

### Résztvevői profil

- **Kor:** 34
- **Munkakör:** DevOps / platform engineer
- **Tapasztalat:** 8 év üzemeltetés, CI/CD, belső API-k, service-to-service auth és logelemzés
- **Munkahelyi környezet:** több környezetet kezel: local, dev, staging, production; gyakran vizsgál hibákat incidens vagy release előtt
- **Technikai rutin:** terminálban dolgozik, HTTPie-t és cURL-t használ, GUI klienseket ritkán nyit meg
- **Tesztelési helyzet:** deployment után ellenőrzi, hogy internal API-k válaszolnak-e, megfelelő auth móddal és environment változókkal

### Kutatási jegyzet

**Megfigyelés:** G-04 gyorsan használta volna a terminálos API klienst, de azonnal rákérdezett arra, hogyan kezeli a program a secret értékeket, tokeneket és environment konfigurációkat. Számára az aktív környezet félreérthetetlen megjelenítése biztonsági kérdés is.

**Idézet:** „Production ellenőrzésnél nem fér bele, hogy rossz environmentre küldjek requestet, vagy token látszódjon logban.”

**Pain pointok:**

- environment tévesztés release vagy incidens közben
- secret értékek véletlen megjelenése
- cURL gyors, de kevés vizuális állapotot mutat
- auth context hiánya miatt lassú hibaelemzés

**TermAPI-hoz kapcsolódó igények:**

- erős felső státuszsáv environment és auth jelzéssel
- secret maszkolás alapértelmezetten
- request export cURL formátumba ellenőrizhető módon
- 4xx/5xx státuszok gyors vizuális és szöveges jelölése
- request context mentése debugginghoz

**Tervezési következtetés:** G-04 workflow-ja igazolja, hogy a TermAPI ne függjön konkrét terminál háttérszíntől, és minden kritikus állapotot szöveges badge is jelezzen. A szín csak kiegészítő visszajelzés legyen, ne egyetlen információhordozó.

---

# Interjúk részletes jegyzetei

## Interjú 1 – Junior backend fejlesztő

**Résztvevő:** Generált Résztvevő G-01  
**Időtartam:** 18 perc  
**Feladat:** lokális endpoint tesztelése, majd ugyanazon request futtatása staging környezetben

### Megfigyelések

- A lokális request gyorsan sikerült.
- Staging váltás után a résztvevő nem ellenőrizte rögtön a tokent.
- A 401 hibát először backendhibának gondolta.
- A request context panel megmutatása után gyorsabban azonosította a problémát.

### Következtetés

Az environment és auth állapotot mindig láthatóvá kell tenni. A hibakeresési nézetben nem elég a response body; aktív environment, auth mód és tokenállapot is szükséges.

## Interjú 2 – Full-stack fejlesztő

**Résztvevő:** Generált Résztvevő G-02  
**Időtartam:** 20 perc  
**Feladat:** endpoint keresés, response ellenőrzés, staging és production response összehasonlításának elképzelése

### Megfigyelések

- Az endpoint browser gyorsan értelmezhető volt.
- A response summary hasznosnak bizonyult, főleg status, time és size értékekkel.
- A résztvevő külön igényelte a diff funkciót.
- Secret maszkolást alapértelmezett működésként várta el.

### Következtetés

A response viewernek egyszerre kell gyors áttekintést és részletes JSON olvasást adnia. A staging / production diff későbbi fejlesztési irányként erős validációt kapott.

## Interjú 3 – Informatikus hallgató

**Résztvevő:** Generált Résztvevő G-03  
**Időtartam:** 16 perc  
**Feladat:** OpenAPI alapján endpoint keresése és request összeállítása

### Megfigyelések

- A résztvevő Swagger UI-ban könnyen tájékozódott, de cURL-ben hibázott.
- A request builder mezői csökkentették a hibázás esélyét.
- A shortcutokat csak akkor használta magabiztosan, ha az alsó sávban látta őket.
- A színek önmagukban nem voltak elég informatívak; a szöveges badge-ek segítettek.

### Következtetés

A TermAPI-nak kezdő felhasználók számára is tanulható TUI-t kell nyújtania. Kontextusfüggő shortcut bar és világos panelcímek szükségesek.

---

# Felületértékelési jegyzőkönyv

## Résztvevők

- 2 fejlesztő: Generált Résztvevő G-01, Generált Résztvevő G-02
- 1 hallgató: Generált Résztvevő G-03

## Feladatok

1. Endpoint keresése OpenAPI import után.
2. Request összeállítása path param, query param és auth mezőkkel.
3. Request futtatása.
4. 401 hiba okának azonosítása.
5. Request exportálása cURL formátumba.

## Eredmények

| Feladat               | Eredmény            | Megjegyzés                                                     |
| --------------------- | ------------------- | -------------------------------------------------------------- |
| Endpoint keresés      | sikeres             | endpoint browser egyértelmű volt                               |
| Request összeállítása | részben sikeres     | kezdő résztvevőnek kellett shortcut segítség                   |
| Request futtatása     | sikeres             | `r` shortcut gyors volt                                        |
| 401 hiba javítása     | sikeres segítséggel | request context panel hasznosnak bizonyult                     |
| cURL export           | sikeres             | fejlesztők fontosnak tartották megosztáshoz és dokumentáláshoz |

## Visszajelzések alapján módosított elemek

| Probléma                            | Módosítás                                                     | UX indoklás           |
| ----------------------------------- | ------------------------------------------------------------- | --------------------- |
| Environment nem volt eléggé feltűnő | felső sávba állandó `Env` badge került                        | hibamegelőzés         |
| Shortcutok nehezen tanulhatók       | minden képernyő alján kontextusfüggő shortcut bar jelenik meg | tanulhatóság          |
| 401 hiba oka nem volt világos       | Error Debugging nézetbe Request Context panel került          | gyorsabb diagnosztika |
| Secret értékek láthatók lehettek    | alapértelmezett maszkolás                                     | biztonság és bizalom  |

---

# Összegzés

A kutatás alapján a TermAPI célcsoportja olyan fejlesztőkből és informatikus hallgatókból áll, akik API-kat gyakran tesztelnek fejlesztés, integráció vagy hibakeresés közben. A fő probléma nem az, hogy ne lenne elérhető API kliens, hanem hogy a meglévő eszközök vagy túl GUI-központúak, vagy túl kevés vizuális és diagnosztikai contextet adnak.

A validált tervezési irány:

- terminálban maradó workflow
- billentyűzetközpontú navigáció
- endpoint browser és strukturált request builder
- response viewer status/time/size összefoglalóval
- Error Debugging nézet request contexttel
- mindig látható environment és auth állapot
- secret értékek maszkolása
- szín mellett szöveges státusz badge-ek

A kutatás megerősítette a TermAPI piaci rését: Postman-szerű struktúra, cURL-szerű gyorsaság és TUI-alapú hibakeresési támogatás egy eszközben.
