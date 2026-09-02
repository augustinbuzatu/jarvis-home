# Probleme intalnite si rezolvari

Bug-uri reale, gasite si rezolvate pe parcursul dezvoltarii. Utila pentru oricine reia sau depaneaza sistemul.

## Retea

**Home Assistant genereaza URL-uri catre adresa interna Docker, nu catre adresa reala de retea**
Raspunsurile TTS nu se auzeau deloc, desi log-urile confirmau redare cu succes. Cauza: adresa "automata" din Settings -> System -> Network nu s-a actualizat corect dupa schimbarea de retea a hub-ului.
Fix: Settings -> System -> Network -> "Home Assistant URL" -> dezactivat switch-ul "Automatic" -> setata manual adresa locala reala.

**Serviciul LVA nu detecteaza schimbari de IP in timp ce ruleaza**
Anunta adresa doar la pornire. Daca IP-ul se schimba (lease DHCP reinnoit), satelitul devine inaccesibil.
Fix: `sudo systemctl restart linux-voice-assistant` dupa orice schimbare de retea. Solutie definitiva: IP static rezervat in router (inca nefacut - vezi `roadmap.md`).

**Doua interfete de retea active simultan pe Raspberry Pi** (eth0 + wlan0)
WiFi-ul ramas configurat din scrierea initiala a cardului SD s-a activat de la sine, cauzand doua IP-uri diferite si confuzie.
Fix: `sudo rfkill block wifi` pe Pi, ramane doar ethernet.

**Adresa Wyoming adaugata manual cu IP-ul intern Docker in loc de IP-ul real** (varianta veche, wyoming-satellite)
Bucla infinita "initializing -> fail" dupa fiecare restart, pentru ca adresa interna se schimba la fiecare repornire a containerului.
Fix: folosita adresa IP de LAN, stabila.

## Audio

**PipeWire redirectiona iesirea audio catre microfon in loc de boxe**
Fara nicio eroare vizibila - log-urile arata "Playing" cu succes, dar nu se auzea nimic. Cauza: sink-ul implicit PipeWire era setat pe interfata audio USB a microfonului, nu pe iesirea jack 3.5mm (boxele reale).
Fix: `wpctl status` ca sa identifici sink-ul corect, apoi `wpctl set-default <id>` + `sudo systemctl restart linux-voice-assistant`.

**Add-on-ul comunitar Wyoming Satellite (pe HAOS) nu avea acces la hardware-ul audio pentru output**
Testat exhaustiv: `paplay` (cu si fara flag-uri raw), `aplay` direct pe ALSA - toate esuate cu erori diferite. Concluzie: containerul avea acces doar la microfon (prin PulseAudio), nu si la iesirea audio.
Fix: nu exista, in configurarea add-on-ului. Solutia a fost renuntarea completa la rularea satelitului ca add-on HAOS, mutat pe Raspberry Pi OS Lite nativ.

## Voce / STT / Wake word

**Vocea custom (Sanda) nu aparea in dropdown-ul de Voice, desi era instalata corect**
Cauza: eticheta de limba a vocii ("Romanian") nu se potrivea cu limba setata in pipeline (`ro_RO`).
Fix: schimbata limba pipeline-ului explicit pe "Romanian" (nu codul de limba).

**Wake word "okay_nabu" fara pauza inainte de comanda -> audio amestecat in transcriere**
Transcrieri garbled (ex. "Ok, inabu, opreste luminat").
Fix: pauza scurta dupa wake word, inainte de a incepe comanda.

**Auto gain prea mare -> satelitul asculta la infinit** (zgomotul de fundal amplificat interpretat ca vorbire continua)

**Auto gain redus prea mult -> transcrieri goale/eronate** (semnal prea slab pentru Whisper)

Fix pentru ambele: ajustare manuala, gasit un punct de echilibru intre cele doua extreme.

**Model Whisper `tiny-int8` (necesar pe Raspberry Pi din cauza RAM) -> acuratete slaba pentru romana**
Fix: mutat Whisper pe laptop (mai multa RAM disponibila, model mai mare posibil).

## Resurse / stocare

**Tot pipeline-ul pe Raspberry Pi 4 (2GB RAM) -> instabil**
HAOS + Whisper + Piper + openWakeWord + Wyoming Satellite simultan depaseau RAM disponibila.
Fix: HAOS + Whisper + Piper mutate pe laptop, Pi-ul ramas doar ca satelit (microfon + boxe).

**Card SD aproape plin (14GB total)**
Fix: sters foldere nemaifolosite (`wyoming-satellite`, `wyoming-openwakeword`) + `apt clean`/`apt autoremove`.

## Consola / unelte

**`wget` lipseste din consola host HAOS** (sistem minimal)
Fix: foloseste `curl -L -o fisier URL` in loc.
