# Ötéves tervünk – beállítás

Közös tervező webapp (ház, kert, utazások, 2026–2030). A fájlok GitHub Pages-en futnak,
az adatok a Google Firebase ingyenes adatbázisában (Firestore) vannak. Belépni Google-fiókkal
lehet, és csak a két megadott e-mail-cím fér hozzá. Telefonra appként telepíthető, és internet
nélkül is megnyílik (a módosítások akkor szinkronizálódnak, amikor újra van net).

Költség: 0 Ft. Két embernek bőven elég a Firebase ingyenes „Spark” csomagja, bankkártya sem kell.

## 1. Firebase-projekt (kb. 10 perc, egyszer)

1. Nyisd meg a **console.firebase.google.com** oldalt, és lépj be a saját Google-fiókoddal.
2. **Create a project / Projekt létrehozása** → név, pl. `otevesterv`. A Google Analytics
   nem kell, kikapcsolhatod.
3. **Adatbázis:** bal oldalt **Build → Firestore Database → Create database**.
   - Edition: **Standard edition**.
   - Database ID: maradjon **`(default)`**, mert az app ezt az adatbázist keresi.
   - Helynek válaszd az `eur3 (europe-west)` vagy az `europe-central2` (Varsó) lehetőséget.
     Ezt később nem lehet megváltoztatni.
   - Ha API-típust is kérdez, a **Firestore Native** legyen, ne a MongoDB.
   - Indítsd **production mode**-ban.
4. **Szabályok:** a Firestore oldalon a **Rules** fül. Töröld ki a tartalmát, és másold be a
   `firestore.rules` fájl tartalmát. **Írd át a két e-mail-címet** a saját Google-címetekre
   (kisbetűvel), majd kattints a **Publish** gombra.
5. **Belépés:** nyisd meg az **Authentication** oldalt. Ha a bal menüben nem találod, írd be a
   felső keresőbe, hogy `Authentication`, vagy nyisd meg közvetlenül:
   `https://console.firebase.google.com/project/PROJEKTAZONOSITO/authentication`.
   → **Get started → Sign-in method → Google** → Enable.
   Adj meg egy támogatási e-mail-címet, majd kattints a Save gombra.
6. **Webapp regisztrálása:** bal oldali menü **Settings** → **General** (Project settings) → lent **Your apps** →
   a `</>` (Web) ikon → név, pl. `otevesterv`. A Firebase Hosting **ne** legyen bepipálva.
   → **Register app**.
7. Megjelenik egy `firebaseConfig = { ... }` blokk. Másold át az értékeit a
   **`firebase-config.js`** fájlba az `IDE_JON_...` és a `PROJEKTNEV` helyére.
   (Ez nem titkos adat, nyugodtan felkerülhet a GitHubra. A védelmet a 4. lépés szabályai adják.)

## 2. Feltöltés GitHub Pages-re

Ugyanúgy megy, mint a mahjongnál:

1. A github.com-on hozz létre egy új tárolót (repository), pl. `otevesterv` néven, **Public**
   beállítással. Az ingyenes Pages csak publikus tárolóval működik, de az adatok így sem
   láthatók senkinek, csak az app kódja.
2. **Add file → Upload files**: töltsd fel a mappa teljes tartalmát (`index.html`, `sw.js`,
   `manifest.webmanifest`, `firebase-config.js`, `icons` mappa). A `firestore.rules` és ez a
   fájl nem kell, de nem is árt.
3. **Settings → Pages** → Source: `Deploy from a branch`, Branch: `main` / `(root)` → Save.
4. Egy-két perc múlva elérhető itt: `https://FELHASZNALONEVED.github.io/otevesterv/`

## 3. A GitHub-cím engedélyezése a belépéshez

Firebase konzol → **Authentication → Settings → Authorized domains → Add domain** →
`FELHASZNALONEVED.github.io` (csak a domain, `https://` és útvonal nélkül).

Enélkül a Google-belépés `auth/unauthorized-domain` hibát ad.

## 4. Első indítás

1. Nyisd meg a címet, és lépj be a Google-fiókoddal.
2. Az üres táblán a **Kiinduló lista betöltése** gombbal bekerül a 14 kezdő tétel
   (kerítés, szigetelés, konyha, ...).
3. Küldd el a címet a párodnak. Ő a saját Google-fiókjával lép be. A címének szerepelnie kell a
   szabályokban (1/4. lépés).

## Telefonra telepítés

- **Android (Chrome):** nyisd meg a címet → ⋮ menü → **Alkalmazás telepítése** vagy
  **Hozzáadás a kezdőképernyőhöz**.
- **iPhone (Safari):** Megosztás gomb → **Főképernyőhöz adás**.

Ha iPhone-on a kezdőképernyőről indított appban nem nyílik meg a Google-belépés ablaka,
lépj be egyszer Safariban, és próbáld újra az appból. Végső esetben a Safari-lapot is
használhatjátok.

## Ha valaki más is kap hozzáférést

Firestore → **Rules**: vedd fel az e-mail-címét a listára, majd kattints a **Publish** gombra.
Az app kódját nem kell módosítani.

## Mentés, biztonsági másolat

Az adatok a Firebase-ben vannak, nem a telefonon, így telefoncsere után is megmaradnak.
Kézi mentéshez: Firestore → Data → `items` gyűjtemény. Ha kell, kérj hozzá egy
„Exportálás CSV-be” gombot az appba.

## Hibák

| Mit látsz | Mit csinálj |
|---|---|
| „Az app még nincs összekötve az adatbázissal.” | Töltsd ki a `firebase-config.js` fájlt (1/7. lépés). |
| `auth/unauthorized-domain` | Add hozzá a GitHub-domaint (3. lépés). |
| „Ez a fiók … nincs a hozzáférési listán.” | Az e-mail-cím hiányzik a szabályokból, vagy elírtad (1/4. lépés). |
| Nem látszik egy friss módosítás | Frissítsd az oldalt. Az app mindig a legújabb verziót tölti le, ha van net. |
