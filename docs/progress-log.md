# Jurnal de progres

Sesiuni de lucru, cele mai recente primele. Fiecare sesiune a pornit dintr-un document de "handoff" folosit ca sa se continue discutia cu asistentul AI intr-un chat nou, dupa ce contextul anterior s-a pierdut.

## Sesiune 4 (cea mai recenta)

**Tema principala:** probleme de retea (hub-ul a ramas fara cablu ethernet) si rezolvarea lor in lant, plus revenirea vocii Sanda dupa o problema de configurare.

- Hub-ul a ramas fara cablu ethernet fizic disponibil - configurat WiFi direct din consola host HAOS (`ha network update ...`), fara sa fie nevoie de USB de configurare
- Decizie de arhitectura: singurul cablu ethernet disponibil a ramas pe satelit (Pi), mai sensibil la latenta pentru audio live; hub-ul a trecut pe WiFi (compromis acceptat, desi HA recomanda oficial ethernet)
- Bug subtil gasit: raspunsurile TTS nu se auzeau deloc, desi log-urile arata "Playing" cu succes - cauza: Home Assistant genera URL-uri catre adresa interna Docker (`172.30.32.1`), nu adresa reala de retea a hub-ului. Rezolvat prin setare manuala a adresei
- Vocea Sanda a disparut din dropdown-ul de Voice dupa reinstalare - cauza: eticheta de limba a vocii nu se potrivea cu limba pipeline-ului. Rezolvat prin setarea explicita a limbii pipeline-ului
- Descoperire utila: consola fizica HAOS (`login` la promptul `ha >`) da acces complet la shell bash, util pentru orice depanare care nu tine de add-on-uri

**Status la final:** bucla completa de comanda vocala functionala, cu vocea Sanda. Ramase: stabilitate retea pe termen lung, configurare Music Assistant.

## Sesiune 3

**Tema principala:** migrare completa de la wyoming-satellite la Linux Voice Assistant (LVA), ca sa se poata folosi media player nativ (necesar pentru redare muzica).

- wyoming-satellite (folosit anterior) nu suporta media_player si e oficial neintretinut - migrat la LVA, care foloseste protocolul ESPHome si suporta media nativ
- Bug gasit: PipeWire redirectiona iesirea audio catre microfon in loc de boxe, fara nicio eroare vizibila in log - cauza: sink-ul implicit PipeWire era setat gresit pe interfata audio a microfonului USB. Rezolvat cu `wpctl set-default` + restart serviciu
- Card SD aproape plin (14GB total) - eliberat spatiu stergand folderele wyoming-satellite/wyoming-openwakeword, nemaifiind necesare
- Rezultat: bucla completa de comanda vocala functionala end-to-end, latenta ~1-2 secunde

**Status la final:** cerere de baza functionala. Ramase: conectare Music Assistant la o sursa de muzica, zona (Area) a satelitului setata gresit pe "Bedroom".

*(Sesiunea 2 - migrarea initiala de pe Raspberry Pi 4 catre setup cu hub separat - nu are un document de handoff pastrat separat; contextul ei relevant apare rezumat in sesiunile 1 si 3.)*

## Sesiune 1 (primul prototip functional)

**Tema principala:** primul prototip end-to-end functional - wake word + STT + procesare locala + TTS, plus prima incercare (esuata) de satelit pe Raspberry Pi.

- Pipeline complet (HAOS + Whisper + Piper + openWakeWord) mutat de pe Raspberry Pi 4 (2GB RAM, insuficient pentru tot pipeline-ul simultan) pe laptop
- Assist pipeline "Asistent RO" creat si confirmat functional (raspuns corect la "Cat este ora?")
- Incercare esuata, testata exhaustiv: redare audio prin add-on-ul comunitar Wyoming Satellite pe HAOS - containerul nu avea deloc acces la hardware-ul audio real pentru output (doar la microfon). Concluzie: bug nerezolvabil prin configurare, specific arhitecturii sandboxed a Supervisor-ului HAOS
- Decizie rezultata: satelitul (microfon + boxe) mutat sa ruleze separat, pe Raspberry Pi OS Lite nativ, nu ca add-on HAOS

**Status la final:** prototip de baza functional pe laptop; satelitul pe Raspberry Pi ramas de facut de la zero, cu arhitectura noua.
