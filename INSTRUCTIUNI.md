# Money Run - cum il pui pe telefon

Jocul e gata si ruleaza in `index.html`. Ai doua cai sa il ai ca aplicatie pe Android.
Codul de joc e acelasi in ambele.

Fisiere in pachet:
- index.html              (jocul propriu-zis, single file)
- manifest.webmanifest    (date aplicatie PWA)
- sw.js                   (service worker - cache offline)
- icon-192.png / icon-512.png / icon-512-maskable.png  (iconite)

------------------------------------------------------------
VARIANTA 1 - PWA (cel mai rapid, fara unelte, recomandata pt inceput)
------------------------------------------------------------
Instalezi aplicatia direct din browserul telefonului. Apare cu iconita,
ruleaza fullscreen si merge si offline. Trebuie servita peste HTTPS
(GitHub Pages e perfect - folosesti deja).

Pasi:
1. Pune toate fisierele intr-un repo GitHub (ex: money-run).
2. Settings -> Pages -> Source: Deploy from a branch -> branch main / root.
3. Dupa cateva minute ai un link de tip:
      https://<user>.github.io/money-run/
4. Deschizi linkul pe telefon in Chrome.
5. Meniu (cele 3 puncte) -> "Instaleaza aplicatia" / "Adauga la ecranul principal".
6. Gata - apare iconita Money Run pe telefon ca o aplicatie normala.

Nota: din browser pe desktop poti testa imediat, dar instalarea ca app
si service worker-ul merg doar pe http/https, nu din fisier local (file://).
Pentru test local rapid: in folder ruleaza `python -m http.server` si deschizi
http://localhost:8000

------------------------------------------------------------
VARIANTA 2 - APK real (Capacitor) - pentru .apk / Play Store
------------------------------------------------------------
Acelasi joc, impachetat intr-o aplicatie nativa. Scoti un .apk pe care il
instalezi prin sideload sau il urci pe Google Play. Ai nevoie de Node.js si
Android Studio instalate pe PC (le ai ca dezvoltator).

  # 1. proiect nou
  npm init -y
  npm install @capacitor/core @capacitor/cli @capacitor/android
  npx cap init "Money Run" com.exemplu.moneyrun --web-dir=www

  # 2. pune fisierele jocului in folderul www/
  #    (index.html, manifest.webmanifest, sw.js, icon-*.png)
  mkdir www
  # copiaza fisierele aici

  # 3. adauga platforma Android si sincronizeaza
  npx cap add android
  npx cap sync android

  # 4. deschide in Android Studio si fa Build
  npx cap open android

In Android Studio:
- Build > Build Bundle(s)/APK(s) > Build APK(s)  -> scoti .apk de test
- pentru Play Store: Build > Generate Signed Bundle/APK -> .aab semnat

------------------------------------------------------------
RECLAME / MONETIZARE (optional, mai tarziu)
------------------------------------------------------------
In cod, functia revive() (butonul "Continua o data") e pregatita ca placeholder
pentru o reclama reward. Cu Capacitor poti lega un plugin de AdMob:
arati reclama, iar in callback-ul de "reward primit" apelezi revive().
La fel poti pune reclame intre runde (la gameOver) sau optiunea de a le elimina
prin plata (un flag salvat care sare peste afisarea reclamei).

------------------------------------------------------------
CE E IN JOC ACUM
------------------------------------------------------------
- 3 benzi, personaj care alearga automat
- controale: swipe stanga/dreapta = banda, sus = sari, jos = apleaca, tap = sari
  (pe desktop: sageti / WASD / Space, P = pauza)
- obstacole: cutii, garduri, masini, bariere (apleaca-te), gropi (sari)
- colectabile: monede (+1) si bancnote (+5)
- power-up-uri: 🧲 magnet, 🛡 scut, ⚡ boost (invincibil), ✖️ x2 bani, 🪂 saritura inalta
- scor din distanta + bani, dificultate care creste (viteza + densitate obstacole)
- meniu: Play / Magazin / Setari / cel mai bun scor
- magazin cosmetic: 8 personaje (culori) + 5 trail-uri
- game over: scor, distanta, bani runda, record + Reia / Meniu / Continua o data
- sunete sintetizate (fara fisiere) + muzica de fundal, vibratii
- salvare locala: best score, bani totali, skin-uri, setari

------------------------------------------------------------
USOR DE EXTINS
------------------------------------------------------------
Constantele de tuning sunt sus in script (CONFIG): viteza, gravitatie,
durate power-up, distanta de spawn, dificultate. Obstacolele sunt definite
in obiectul OBST, iar tiparele de aparitie in functia spawnPattern() -
adaugi acolo misiuni, harti noi, evenimente sezoniere sau power-up-uri noi.
