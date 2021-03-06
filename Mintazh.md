### 1. Hogyan reprezentálhatóak fizikai rendszerek modelljei diagrammok segítségével? Mik lesznek a diagrammok élei, csomópontjai, mi a szerepük a forrásoknak és a nyelőknek? Honnan származhatnak a modellek egyenleti?
 - Reprezentáció: A fizikai rendszereket leíró differenciálegyenletet szimulálni kell valamilyen numerikus algoritmussal. Ezek gráfként reprezentálhatóak, amelyekben:
	 - élek: Irányítottak, blokkok bemeneteit kötik össze más blokkok kimeneteivel. Pillanatnyi értékük lehet skalár, vagy vektor (időben változó). Azonos típusú és dimenziójú ki- és bemeneteket kötnek össze
	 - Csómópontok/blokkok: Jelek közti (akár dinamikus) átalakításokat definiálnak. Több bemenettel, több kimenettel rendelkezhetnek, ezek mindegyike lehet vektor. Saját állapottal és paraméterekkel rendelkezhetnek.


- Jelforrás (source) : Bemenet nélküli blokk
- Jelnyelő (sink) : Kimenet nélküli blokk

**Modellek egyenletei**:
1. A fizikai törvényszerűségek alapján felírt differenciálegyenletek szerint adódik. (esetleg munkapont körüli linearizálás útján)
2. A be- és kimeneti jelek  mérése alapján azonosírjuk
---
### 2. Adja meg az algebrai hurok definícióját és sorolja fel a feloldására alkalmas módszereket
Algebrai (vagyis belső állapottal nem rendelkező) blokkokból álló hurok. A be- és kimenetek egymástól való körkörös összefüggése.

**Feloldása**: 
1. Simulink algebrai-hurok megoldó algoritmusának segítségével
		- Van legalább egy blokk a hurokban
		- A modell jelei lebegőpontos valósak
		- A korlátozás folyamatosan differenciálható
2. A modell átstruktúrálásával, bővítésével.
3. Algebrai korlátozás differenciálegyenletté konvertálásával.
4. Késlelteltetés beszúrásával
----
### 3. Ismertesse a differenciálegyenletek megoldására szolgáló Heun-módszert! Adja meg a módszer egyenleteit, Butcher-tábláját ! Mekkora a módszer lokális és globális hibája?
![heun.png](https://i.loli.net/2018/10/29/5bd6159737668.png)
![](https://i.loli.net/2018/10/29/5bd6170077158.png)

---
### 4. Mik a diagram futtatásának főbb lépései a Simulink környezetben és milyen műveletek kerülnek ezeknél végrehajtásra?

A fordítás három fázisa:
1. Kompilálás/fordítás: 
	- Blokkok paramétereinek kiértékelése
	- Jelek attribútumainak ellenőrzése
	- Egyszerűsítések
	- Virtuális alrendszer helyettesítése
	- Blokkok (végrehajtási) sorba rendezése
	- Mintavételi idők regisztrálása
2. Linkelés: Futási időben szükséges erőforrások lefoglalása, metódus híváslisták előállítása 
3. Iterálás: Állapotok, bemenetek, kimenetek ciklikus újraszámlálása, ahogy a szimulálási idő halad előre.

---

### 5. Mi jellemzi a Simulink diagram csomópontjait? Milyen adatstruktúrákra és metódusokra van szükség a csomópontok esetében a futtatáshoz?


Csomópontok jellemzői:
- Bemenetek:
	- dimenzió
	- típus
- Kimenetek:
	- dimenzió
	- típus
	- számítási szabvány
- Paraméterek:
	- típusok
	- értékek
- Folytonos dinamika:
	- állapot dim. & típus
	- kezdeti érték
	- derivált számítási szabálya
- Diszkrét dinamika:
	- állapot dim. & típus
	- kezdeti érték
	- aktualizálás számítási szabálya

---
### 6. Az alábbi diagram a Simulink modellfuttatási végrehajtási hurkát ábrázolja. Töltse ki és magyarázza meg az egyes négyzetekkel reprezentált műveleteket! 

![diagram.png](https://i.loli.net/2018/10/29/5bd6180ba4ee1.png)


A hurokban található műveletek ciklikusan végrehajtódnak. A hurkot a Simulink motorja vezérli. A linkelési fázisban keletkeznek a diagram szintű `derivates`, `update`, `outputs` metódusok. Két mintavételi időpont között a folytonos állapotok alakulását  a kiválasztott `solver` szerint számoljuk (integráljuk).

### 7. Mire szolgál az S-függvény? Milyen metódusok implementálása szükséges? Hogyan lehetséges C nyelvű kódok beépítése?

**S-függvény:** Saját blokkokat (sőt blokk-könyvtárakat hozhatunk létre, saját paraméterhalmazokkal.

Hasznos lehet:
- tetszőleges rendszer leírására egyenletrendszerekkel
- új, általános célú blokkok létrehozására
- hardverspecifikus funkciók megvalósítására
- létező C kód szimulációjára, tesztelésére
- szimuláció gyorsítársára
- vizualizációra

A metódusokat több fajta nyelven is kódolhatjuk. Ezzel gyorsítást érhetünk el és/vagy valós idejű megvalósítást teszünk lehetővé.

Metódusok:
- **Setup** - kompilálást segítő függvény
- **Derivates** - deriváltak számítása
- **Update** - következő állapot számítása
- **Outputs** - kimenetek frissítése
- **InicializeCondition** - kezdeti állapot

__C kódok__: Script készítésével, amelyek kiterjesztése .m lesz 

---
### 8. Mit értünk egy Simulink modell „külső” (external) futtatásán? Milyen fordítási lépések szükségesek a külső futtatáshoz? Milyen korlátozásokat kell betartani egy Simulink modell esetében ahhoz, hogy azt „külső” módban futtathassuk? Magyarázza meg a Target fogalmát!

**"Külső" (external) futtatás:** Az iterálási hurok számításait lehetőségünk van Matlabon "kívül", egy másik számítógépen, vagy egy valós idejű operációs rendszert futtató számítógépen elvégeztetni.

**Fordítási lépések:**
- RTW (Real-Time Workshop) build
- Target Language Compiler
- make (=> model.exe) 
- Letöltés a célszámítógépre
- Végrehajtás Simulink külső módban

**Korlátozások:**
- Valós idő esetén:
	- A hurokban lévő számítások determinisztikusak és jósolhatóak.
	- A folytonos tagok numerikus integrálása NEM függhet adaptív módon a deriváltak értékétől (fix lépésköz)
	- Nem megengedhető dinamikus memóriahasználat vagy nem ismert be és kimeneti dimenzió
	- Nem megengedhető az iteráció

**Target:** Az a számítógép (mikrokontroller), ahol a Simulink modell futtatásra kerül. Ehhez kell egy fordító, amely képes az ún. Target Language-ből végrehajtható kódot előállítani. Ez a fordító tipikusan C/C++ fordító.

---

### 9. Magyarázza meg a ráfutás (overrun) jelenségét egy ábra segítségével akkor, ha a ráfutást egy megszakításvezérelt taszk okozza, illetve akkor ha „Single Timer” módban futtatunk különböző mintavételi periódusidejű blokkokból álló Simulink modelleket!
<!--stackedit_data:
eyJwcm9wZXJ0aWVzIjoiZXh0ZW5zaW9uczpcbiAgcHJlc2V0Oi
Bjb21tb25tYXJrXG4iLCJoaXN0b3J5IjpbOTk2MTU0NTc3LC0y
NjYwOTc0NDAsLTQ4MTgxNTEyMCwxMDQ5MTUyMzk4LC00MjY4Nj
IwMTYsLTEzMDczMTM3OTcsNjE0NTI4MDc1LC0zNzI1MTc5Njld
fQ==
-->