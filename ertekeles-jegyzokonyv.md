# Felületértékelési jegyzőkönyv (szintetikus)

## Projekt

**TermAPI** – billentyűzetközpontú TUI alkalmazás API-k felfedezésére, tesztelésére és hibakeresésére.

## Értékelés célja

Az értékelés célja annak vizsgálata volt, hogy a TermAPI prototípus fő felületei mennyire támogatják az API-tesztelési munkafolyamatot fejlesztők és informatikus hallgatók számára.

Kiemelt vizsgálati szempontok:

- Az endpoint browser szerkezete érthető-e első használatkor.
- A request builder segít-e csökkenteni a path param, query param, header és auth hibákat.
- A response viewer gyorsan értelmezhető összefoglalót ad-e státuszról, válaszidőről, méretről és body tartalomról.
- Az Error Debugging nézet segít-e azonosítani a 401 hiba valós okát.
- Az aktív environment és auth állapot eléggé feltűnő-e.
- A shortcutok tanulhatók-e állandó segítség nélkül.
- A secret értékek maszkolása növeli-e a biztonságérzetet.

## Módszertan

### Résztvevők

3 résztvevő vett részt a prototípus értékelésében:

- 2 fejlesztő
- 1 informatikus hallgató

A résztvevők profiljai a korábbi kutatási jegyzőkönyvben szereplő generált felhasználói mintákhoz hasonlóan készültek: fejlesztési háttér, API-tesztelési rutin, tipikus hibák és workflow-környezet alapján.

### Tesztelt prototípus-elemek

- endpoint browser
- request editor / request builder
- response viewer
- Error Debugging nézet
- felső státuszsáv `Env` és `Auth` badge-ekkel
- alsó kontextusfüggő shortcut bar
- secret maszkolás
- cURL export

### Feladatok

1. Endpoint keresése OpenAPI import után.
2. Request összeállítása path param, query param és auth mezőkkel.
3. Request futtatása billentyűparanccsal.
4. 401 hiba okának azonosítása az Error Debugging nézetben.
5. Request exportálása cURL formátumba.

### Adatgyűjtés

- megfigyelés feladatvégzés közben
- rövid utóinterjú
- sikeresség és elakadások jegyzése
- résztvevői idézetek rögzítése
- tervezési következmények összegzése

---

# Résztvevői profilok és értékelési jegyzetek

## 1. Generált Értékelő E-01

### Résztvevői profil

- **Kor:** 25
- **Munkakör:** junior backend fejlesztő
- **Tapasztalat:** 2 év JavaScript és Python backend fejlesztés, REST API endpointok kézi tesztelése lokális és staging környezetben
- **Munkahelyi környezet:** kis fejlesztőcsapat, ahol új endpointokat gyakran kell gyorsan ellenőrizni release előtt
- **Technikai rutin:** napi terminálhasználat, Postman használata autentikációs és staging tesztekhez
- **Tesztelési helyzet:** lokális `GET /courses/{id}` endpointot keres, majd staging környezetben futtatja bearer tokennel

### Értékelési jegyzet

**Megfigyelés:** E-01 gyorsan megtalálta az endpointot az endpoint browserben, és a request builderben egyértelműnek tartotta a path param és query param mezőket. A request futtatása sikerült az `r` shortcut használatával. Stagingre váltás után 401 hibát kapott, de először nem nézte meg az aktív `Env` badge-et.

**Idézet:** „A hiba nézet jó, mert nem csak azt látom, hogy 401, hanem azt is, milyen tokennel és környezettel ment ki a request.”

**Feladatok eredménye:**

| Feladat               | Eredmény            | Megjegyzés                                              |
| --------------------- | ------------------- | ------------------------------------------------------- |
| Endpoint keresés      | sikeres             | endpoint browser egyértelmű volt                        |
| Request összeállítása | sikeres             | path param és auth mezők jól elkülönültek               |
| Request futtatása     | sikeres             | `r` shortcut gyorsan megtanulható volt                  |
| 401 hiba javítása     | sikeres segítséggel | request context panel után azonosította a lejárt tokent |
| cURL export           | sikeres             | dokumentációhoz hasznosnak tartotta                     |

**Pain pointok:**

- az aktív environment elsőre nem volt elég feltűnő
- 401 hiba esetén először backendhibára gyanakodott
- tokenállapot ellenőrzésére külön vizuális jelzés kell
- shortcutokat csak akkor használta magabiztosan, ha látta őket az alsó sávban

**Tervezési következtetés:** E-01 validálta az Error Debugging nézet szükségességét. A felső státuszsávban az `Env` jelzést erősebben kell kiemelni, mert a hibamegelőzéshez nem elég, ha csak jelen van.

---

## 2. Generált Értékelő E-02

### Résztvevői profil

- **Kor:** 31
- **Munkakör:** full-stack fejlesztő
- **Tapasztalat:** 6 év React, TypeScript és backend integráció; API response struktúrák ellenőrzése frontend fejlesztés előtt
- **Munkahelyi környezet:** SaaS termékcsapat, ahol staging és production válaszok összehasonlítása gyakori feladat
- **Technikai rutin:** Postman, cURL és böngészős devtools magabiztos használata; terminálos workflow-t előnyben részesíti gyors ellenőrzésekhez
- **Tesztelési helyzet:** frontend integráció előtt ellenőrzi a `GET /courses/CS101` response body tartalmát, majd exportálja a requestet megosztáshoz

### Értékelési jegyzet

**Megfigyelés:** E-02 az endpoint browsert és response viewert gyorsan értelmezte. A status, time, size és JSON body egy képernyőn való megjelenítését hasznosnak tartotta. A secret maszkolást külön pozitívumként említette, mert screen share közben gyakran mutat API klienseket. A staging / production diff funkciót hiányolta, de elfogadta későbbi fejlesztési irányként.

**Idézet:** „A response summary pont elég gyors áttekintéshez. Nekem még az lenne fontos, hogy két környezet válaszát gyorsan diffeljem.”

**Feladatok eredménye:**

| Feladat               | Eredmény | Megjegyzés                                     |
| --------------------- | -------- | ---------------------------------------------- |
| Endpoint keresés      | sikeres  | keresés és lista struktúra gyors volt          |
| Request összeállítása | sikeres  | mezők sorrendje logikus volt                   |
| Request futtatása     | sikeres  | billentyűzetes flow gyorsabbnak tűnt GUI-nál   |
| 401 hiba javítása     | sikeres  | request context panel hasznos volt             |
| cURL export           | sikeres  | megosztás és dokumentálás miatt fontos funkció |

**Pain pointok:**

- history és korábbi requestek keresése nem jelent meg elég hangsúlyosan
- staging / production diff hiánya integrációs munkánál korlát
- secret értékek maszkolását kikapcsolhatatlan alapértelmezésként várja el
- keyboard flow jó, de shortcut discoverability továbbra is kritikus

**Tervezési következtetés:** E-02 megerősítette, hogy a response viewer státusz-, idő- és méretösszefoglalója helyes irány. A diff funkció és kereshető history továbbfejlesztési prioritásként jelenik meg.

---

## 3. Generált Értékelő E-03

### Résztvevői profil

- **Kor:** 22
- **Munkakör:** informatikus hallgató, backend gyakornok
- **Tapasztalat:** egyetemi REST API projektek, alap Swagger UI és cURL használat; komplexebb auth flow-kban kevés tapasztalat
- **Munkahelyi környezet:** gyakornoki feladatoknál OpenAPI specifikáció alapján tesztel endpointokat
- **Technikai rutin:** terminált használ, de hosszabb cURL parancsoknál és több header esetén bizonytalan
- **Tesztelési helyzet:** OpenAPI import után endpointot keres, kitölti a path paramot, query paramot és bearer auth mezőt

### Értékelési jegyzet

**Megfigyelés:** E-03 az endpoint listában jól tájékozódott, de a request futtatásához először kereste, melyik billentyűt kell használni. Az alsó shortcut bar alapján megtalálta az `r` futtatási lehetőséget és a `?` segítséget. A 401 hiba nézetben a „missing token”, „expired token” és „wrong environment” jellegű lehetséges okokat hasznosnak tartotta.

**Idézet:** „Ha alul mindig látom, mit lehet nyomni, akkor nem érzem úgy, hogy meg kell jegyeznem mindent.”

**Feladatok eredménye:**

| Feladat               | Eredmény            | Megjegyzés                                       |
| --------------------- | ------------------- | ------------------------------------------------ |
| Endpoint keresés      | sikeres             | endpoint browser tanulható volt                  |
| Request összeállítása | részben sikeres     | auth mezőnél rövid magyarázatra volt szükség     |
| Request futtatása     | sikeres segítséggel | shortcut bar alapján használta az `r` billentyűt |
| 401 hiba javítása     | sikeres segítséggel | javasolt okok segítették a diagnózist            |
| cURL export           | sikeres             | hasznosnak látta tanulási célra is               |

**Pain pointok:**

- shortcutok első használatkor nem maguktól értetődők
- auth típusokhoz rövid magyarázat kell
- színek önmagukban nem elegendők; szöveges badge is szükséges
- komplexebb request body szerkesztéshez további validáció kellene

**Tervezési következtetés:** E-03 alapján a TermAPI tanulhatóságát az állandó shortcut bar, világos panelcímek és szöveges státusz badge-ek biztosítják. A TUI gyorsasága csak akkor előny, ha a kezdő felhasználó nem érzi rejtettnek az interakciókat.

---

# Összesített eredmények

## Feladat-sikeresség

| Feladat               | Összesített eredmény | Értelmezés                                                    |
| --------------------- | -------------------- | ------------------------------------------------------------- |
| Endpoint keresés      | sikeres              | mindhárom résztvevő megtalálta a kijelölt endpointot          |
| Request összeállítása | részben sikeres      | fejlesztők önállóan, hallgató segítséggel teljesítette        |
| Request futtatása     | sikeres              | az `r` shortcut gyors volt, de discoverability kellett        |
| 401 hiba javítása     | sikeres segítséggel  | Error Debugging nézet hasznos volt, de context kiemelése kell |
| cURL export           | sikeres              | fejlesztők megosztáshoz, hallgató tanuláshoz értékelte        |

## Fő megállapítások

- Az endpoint browser egyértelmű volt mindhárom résztvevő számára.
- A request builder csökkentette a cURL-szerű szintaktikai hibák esélyét.
- A response viewer gyors áttekintést adott a status, time, size és body adatokkal.
- Az aktív environmentet több résztvevő későn vette észre.
- A 401 hiba nézetet minden résztvevő hasznosnak ítélte.
- A shortcutok tanulásához állandó, kontextusfüggő segítség kellett.
- A secret maszkolás biztonsági és bizalmi szempontból fontos validált igény.

## Visszajelzések alapján módosított elemek

| Probléma                            | Módosítás                                                     | UX indoklás                                                    |
| ----------------------------------- | ------------------------------------------------------------- | -------------------------------------------------------------- |
| Environment nem volt eléggé feltűnő | felső sávba állandó, kontrasztosabb `Env` badge került        | hibamegelőzés, rossz környezetbe küldött requestek csökkentése |
| Shortcutok nehezen tanulhatók       | minden képernyő alján kontextusfüggő shortcut bar jelenik meg | tanulhatóság és gyorsabb billentyűzetes flow                   |
| 401 hiba oka nem volt világos       | Error Debugging nézetbe Request Context panel került          | gyorsabb diagnosztika, kevesebb bizonytalanság                 |
| Secret értékek láthatók lehettek    | alapértelmezett maszkolás került bevezetésre                  | biztonság és bizalom screen share vagy dokumentálás közben     |

## Tervezési következmények

- A TermAPI fő értéke nem csak request küldés, hanem request context értelmezése.
- Minden kritikus állapotot szín mellett szöveges badge-nek is jeleznie kell.
- Az `Env` és `Auth` állapotoknak folyamatosan láthatónak kell maradniuk.
- Az Error Debugging nézetnek konkrét ellenőrzési irányokat kell adnia, nem csak státuszkódot.
- A shortcut bar nem opcionális segítség, hanem alapvető tanulhatósági elem.
- A cURL export támogatja a fejlesztői dokumentálást és request megosztást.

# Összegzés

Az értékelés alapján a TermAPI prototípus fő szerkezeti döntései validálhatók. Az endpoint browser, request builder és response viewer jól támogatja a fejlesztői API-tesztelési folyamatot. Az Error Debugging nézet különösen erős értéket ad 401/403 típusú hibáknál, mert a státuszkód mellé request contextet és lehetséges okokat mutat.

A legfontosabb fejlesztési irányok az aktív environment erősebb kiemelése, a shortcutok folyamatos láthatósága, a secret értékek alapértelmezett maszkolása és a későbbi staging / production diff funkció támogatása.
