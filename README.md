# Egységes kérelem → AgOpen KML konverter

Egyetlen önálló HTML fájl, amely az **e-kérelem / MePAR** térképszolgáltatás
válaszából kinyeri a parcellahatárokat (EOV koordinátákban), átszámolja azokat
WGS84-re, és letölthető **KML** vagy **Shapefile** formátumban adja vissza —
hogy aztán beimportálhasd pl. **AgOpenGPS**-be vagy bármilyen GIS/navigációs
szoftverbe.

Nincs szükség szerverre, telepítésre vagy build lépésre: egy `.html` fájl,
amit bármelyik böngészőben megnyitva azonnal használható.

## Miért jött létre

A hivatalos e-kérelem felület nem kínál közvetlen KML/Shapefile exportot a
táblahatárokhoz, csak a saját térképnézetét. Ez az eszköz a böngésző
fejlesztői eszközeivel kimásolható nyers szerverválaszból szedi ki a
geometriát, és azt alakítja át hasznosítható fájlformátumokká.

## Főbb funkciók

- **Helyi koordináta-átszámítás** — az EOV (EPSG:23700) → WGS84 átváltás a
  böngészőben, a `proj4js` könyvtárral történik, külső szerverhívás nélkül.
  Ez nagyságrendekkel gyorsabb, mint pontonként egy API-t hívni, és offline
  internetkapcsolat nélkül is működik a feldolgozás.
- **Lyukas (belső határú) poligonok kezelése** — nemcsak a külső határvonalat,
  hanem az esetleges belső kivágásokat (pl. tanya, erdőfolt) is helyesen
  értelmezi.
- **Automatikus területszámítás** hektárban, parcellánként és összesítve.
- **Hibás/gyanús geometria jelzése** (pl. túl kevés pont vagy nulla terület).
- **Térképes előnézet** ingyenes OpenStreetMap csempékkel (MapLibre GL),
  regisztráció vagy API-kulcs nélkül.
- **Exportálási lehetőségek**:
  - egyedi KML parcellánként
  - összes parcella egyetlen KML fájlban
  - egyedi Shapefile (.zip) parcellánként
  - összes parcella egyetlen Shapefile-ban (.zip)
- Szerkeszthető parcellanevek a letöltés előtt.

## Használat

1. Nyisd meg a `ekerelem.html` fájlt egy böngészőben (dupla kattintás elég,
   nincs szükség szerverre).
2. Nyisd meg az e-kérelem MePAR saját adat térképnézetét, és a böngésző
   fejlesztői eszközeivel (F12 → Network/Hálózat fül) másold ki a
   `BizonylatElovalasztoMekirSajatAdat` kérés teljes válaszát.
3. Illeszd be a választ a konverter beviteli mezőjébe, majd kattints a
   **Feldolgozás** gombra.
4. Ellenőrizd a felismert parcellákat (terület, pontszám), szükség esetén
   nevezd át őket.
5. Töltsd le a kívánt formátumot (KML vagy Shapefile), vagy nézd meg a
   parcellákat a beépített térképen.

Részletes, képernyőképes útmutatót a fájlon belüli **„Útmutató”** lenyíló
panel tartalmaz.

## Technikai háttér

| Komponens | Szerep |
|---|---|
| [`proj4js`](https://github.com/proj4js/proj4js) | EOV (EPSG:23700) → WGS84 koordináta-transzformáció helyben |
| [`maplibre-gl`](https://maplibre.org/) | Térképmegjelenítés, ingyenes OSM raszter csempékkel |
| [`shp-write`](https://github.com/mapbox/shp-write) + [`JSZip`](https://stuk.github.io/jszip/) | Shapefile generálás és becsomagolás `.zip`-be, böngészőben |

Az EOV vetületi paraméterek a hivatalos EPSG:23700 (HD72 / EOV) definíción
alapulnak.

## Korlátok

- A bemeneti szöveget a felhasználónak kézzel kell kimásolnia a böngésző
  fejlesztői eszközeiből (nincs automatikus bejelentkezés/lekérdezés a
  hivatalos rendszerbe).
- A térképes előnézethez és a betűtípusokhoz internetkapcsolat szükséges
  (CDN-ről töltődnek be); magához a koordináta-átszámításhoz és az
  exportáláshoz nem.
- A felismerés a jelenleg ismert szerverválasz-formátumra épül
  (`addFeatureToVectorLayer('tter', 'POLYGON (...)', ...)`); ha a portál
  megváltoztatja a választ, a mintaillesztést frissíteni kell.

## Licenc

Szabadon használható és módosítható saját célra.
