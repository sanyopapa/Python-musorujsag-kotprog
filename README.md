# SZTE-Adatbázisok gyakorlat dokumentáció:

![EK](https://github.com/sanyopapa/Python-musorujsag-kotprog/blob/main/E-K%20diagram.png "E-K diagram")

A diagram értelmezése:

Vannak sima felhasználók, róluk nem tárolunk adatot, de csak nézni tudják az adatbázis tartalmát. Az
admin felhasználókról tároljuk az emailjüket, ami a kucs adattagjuk, tehát az egyedi, a nevüket, a
jelszavukat, és az utolsó belépésük időpontját. A bejelentkezett adminok tudják aktualizálni
csatornákat és műsorokat(egy admin sokat, 1:N kapcsolat). A csatornáról tároljuk a nevét, a
kategóriáját és a leírását. A csatornán vetítik a műsorokat, úgyhogy N:M kapcsolatot használtam. A
műsorokról tároljuk a Címüket, hogy hol és mikor vetítik. Ezek határozzák meg pontosan hogy melyik
műsorról beszélünk, mert egy műsort több csatornán, és több időpontban is vetíthetnek. Emellett
tároljuk az epizódjaikat is ami több értékű attribútum mivel több epizódja is lehet egy műsornak. A
műsorokban szerepelnek a szereplők, akikről tároljuk a nevüket, a születési dátumukat, a
foglalkozásukat és a nemzetiségüket. Ez is több értékű attribútum.

# A következő leképzéseket tudjuk létrehozni:

Admin (Email, Nev, Jelszo, Utolso_belepes)

Aktualizál(Email, Nev)

Aktualizál(Email, Cím, Hol)

Csatorna (Nev, Kategoria, Leiras, Email)

Musor (Cim, Sugarzas(Mikor, Hol), Ismerteto, Email, Epizodok (Cim, Epizod), Szereplok (Cim,
Szuletes_datum, Foglalkozas, Nemzetiseg, Nev))

Vetit (Nev, Cim)


Ezután mivel az 1:N kapcsolatot az N oldali egyedbe lehet beolvasztani, és egy táblán belül az azonos
elemek összevonhatóak, így a két Aktualizál tábla eltűnik. A többértékű attribútumoknak külön
táblákat kell létrehoznunk, így a Musor táblából eltűnik az Epizodok, és a Szereplok attribútum, és
létrejön nekik egy-egy külön tábla. Ezekkel a változtatásokkal így néz ki a végleges leképzésünk:

# Relációs adatbázissémák:

Admin (Email, Nev, Jelszo, Utolso_belepes)

Csatorna (Nev, Kategoria, Leiras, Email)

Musor (Cim, Mikor, Hol, Ismerteto, Email)

Epizodok (Cim, Epizod)

Szereplok (Cim, Szuletes_datum, Foglalkozas, Nemzetiseg, Nev)

Vetit (Nev, Cim)

# Funkcionális függőségek:

1. ADMIN tábla:
- {Email} → {Név, Jelszó, Utolsó belépés}
2. CSATORNA tábla:
- {Név} → {kategória, Leírás, Email}
3. MŰSOR tábla:
- {Cím, Mikor, Hol} → {Ismertető, Email}
4. EPIZÓDOK tábla:
- {Cím} → {Epizódok}
5. SZEREPLŐK tábla:
- {Cím, Név} → {Születési dátum, Foglalkozás, Nemzetiség}
6. VETÍT tábla:
- {Név} → {Cím}

# Kulcsok:

1. ADMIN tábla kulcsa:
- Email
2. CSATORNA tábla kulcsa:
- Név
3. MŰSOR tábla kulcsai:


- Cím
- Mikor
- Hol
4. EPIZÓDOK tábla kulcsa:
- Cím
5. SZEREPLŐK tábla kulcsai:
- Cím
- Név
6. VETÍT tábla kulcsa:
- Név

# Normálformák:

1. normálforma (1NF):
- Az összes táblában minden attribútum atomi értékű.

Minden tábla már eleve 1. normálformában van, mert minden attribútum atomi.

2. normálforma (2NF):
- Minden másodlagos attribútum teljesen függ a kulcsoktól.

### - ADMIN:

- Az ADMIN táblában nincs másodlagos attribútum, így 2NF teljesül.

### - CSATORNA:

- A kulcsa a Nev, és minden másodlagos attribútum teljesen függ tőle, tehát 2NF teljesül.

### - MŰSOR:

- A kulcsai: {Cim, Mikor, Hol}
- Az Ismertető és az Email mindkét kulcstól függ, tehát 2NF teljesül.

### - EPIZÓDOK:


- A kulcsa a Cim, és minden attribútum a kulcstól függ, tehát 2NF teljesül.

### - SZEREPLŐK:

- A kulcsai: {Cim, Nev}
- Minden másodlagos attribútum teljesen függ a kulcsoktól, tehát 2NF teljesül.

### - VETÍT:

- A kulcsa a Nev, és minden attribútum a kulcstól függ, tehát 2NF teljesül.
3. normálforma (3NF):
- Az összes tranzitív függés eliminálva van.

### - ADMIN:

- Nincs tranzitív függés, tehát 3NF teljesül.

### - CSATORNA:

- Nincs tranzitív függés, tehát 3NF teljesül.

### - MŰSOR:

- Nincs tranzitív függés, tehát 3NF teljesül.

### - EPIZÓDOK:

- Nincs tranzitív függés, tehát 3NF teljesül.

### - SZEREPLŐK:

- Nincs tranzitív függés, tehát 3NF teljesül.

### - VETÍT:

- Nincs tranzitív függés, tehát 3NF teljesül.

| Tábla      | Mező              | Típus            | Megjegyzés |
|------------|------------------|-----------------|------------|
| **ADMIN**  | Email            | Karaktersorozat | Az admin email címe(kulcs) |
|            | Nev              | Karaktersorozat | Az admin neve |
|            | Jelszo           | Karaktersorozat | Az admin fiókjának a jelszava |
|            | Utolso_belepes   | Időpont         | Az admin utolsó bejelentkezése a fiókjába |
| **CSATORNA** | Nev            | Karaktersorozat | A csatorna neve(kulcs) |
|            | Kategoria        | Karaktersorozat | A csatorna kategóriája |
|            | Leiras           | Karaktersorozat | A csatorna leírása |
|            | Email            | Karaktersorozat | A csatornát felvevő admin email címe(Külső kulcs) |
| **MŰSOR**  | Cim              | Karaktersorozat | A műsor címe(kulcs) |
|            | Mikor            | Dátum           | A műsor sugárzásának időpontja(kulcs) |
|            | Hol              | Karaktersorozat | A csatorna neve, ahol sugározzák |
|            | Ismerteto        | Karaktersorozat | A műsor ismertetője |
|            | Email            | Karaktersorozat | A műsort felvevő admin email címe(külső kulcs) |
| **SZEREPLŐK** | Cim           | Karaktersorozat | A műsor, amiben a szereplő szerepel(külső kulcs) |
|            | Szuletesi_datum  | Dátum           | A szereplő születési dátuma(kulcs) |
|            | Foglalkozas      | Karaktersorozat | A szereplő foglalkozása |
|            | Nemzetiseg       | Karaktersorozat | A szereplő nemzetisége |
|            | Nev              | Karaktersorozat | A szereplő neve(kulcs) |
| **VETÍT**  | Nev              | Karaktersorozat | A csatorna neve, ahol a műsort vetítik(kulcs) |
|            | Cim              | Karaktersorozat | A vetített műsor címe(kulcs) |
| **EPIZÓDOK** | Cim            | Karaktersorozat | A műsor, amihez az epizód tartozik(kulcs) |
|            | Epizod           | Szám            | Az epizód sorszáma(kulcs) |


## A program funkciói:
-Regisztráció, bejelentkezés, kijelentkezés, profil törlése

-Új csatorna létrehozása, meglévő frissítése, meglévő törlése, csatornák kilistázása
különböző kérések szerint

-Új műsor létrehozása, meglévő módosítása, és törlése, létrehozáskor csatornához és
időponthoz rendelése

-Szereplők kilistázása különböző kérések szerint, kiválasztott műsorban lévő szereplők
kilistázása, új szereplő létrehozása, műsorhoz rendelése, meglévő szereplő törlése és
adatainak frissítése

-A műsorokhoz tartozó epizódok kilistázása, műsorok epizód listájának
módosítása(epizódok törlése, és új epizódok létrehozása)


# A program telepítése és használata

A program **Python** nyelven íródott, és **MySQL** adatbázist használ.  

Az **Adatbázis app** nevű mappa **PyCharmban** való megnyitásával, majd a PyCharmba megfelelő csomagok telepítésével, és a **musorujsag.sql** fájl **phpMyAdminban** való importálásával könnyedén üzembe helyezhető.  

Az alkalmazás megnyitásakor egy **vendég nézet** fogad minket, amelyben megtekinthetjük:  
- A csatornákat és adataikat  
- A műsorokat és adataikat  
- A szereplőket és adataikat  
- Bármelyik műsor epizódlistáját  

A **regisztrációs/bejelentkezési (reg/log) gombra** kattintva a regisztrációs és bejelentkező oldal jelenik meg.  

Ha bejelentkezünk, továbbra is elérjük az eddig felsorolt funkciókat, de a **reg/log gomb helyett egy profil gombot** találunk.  
A **profil gombra** kattintva módosíthatjuk a fiókunk **felhasználónevét vagy jelszavát**.  

## További funkciók bejelentkezés után

- **Csatornák kezelése:**  
  - Új csatorna létrehozása  
  - Meglévő csatorna frissítése  
  - Csatorna törlése  

- **Műsorok kezelése:**  
  - Új műsor létrehozása  
  - Műsor frissítése  
  - Műsor törlése  

- **Epizódok kezelése:**  
  - Epizódok frissítése egy meglévő műsorhoz  

- **Szereplők kezelése:**  
  - Adatok bevitele  
  - Szereplő keresése  
  - Szereplő törlése vagy módosítása  
  - Új szereplő létrehozása és hozzárendelése egy műsorhoz  

## Összetett lekérdezések a `main.py` fájlban

- **977. sor - 988. sor**  
- **909. sor**  
- **839. sor**  
- **393. sor**  

