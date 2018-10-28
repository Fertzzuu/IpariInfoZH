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

![heun](heun.png)

>Ide még jön kép heun bácsiról csak még nem tudom hogyan kéne képeket beilleszteni

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

<!--stackedit_data:
eyJoaXN0b3J5IjpbNjE0NTI4MDc1LC0zNzI1MTc5NjldfQ==
-->