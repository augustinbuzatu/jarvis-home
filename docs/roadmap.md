# Roadmap / ce mai ramane de facut

## Prioritate mare

- **Rezerva IP static pentru Raspberry Pi in router** - mentionat repetat, in mai multe sesiuni consecutive, tot amanat. Cauza principala a mai multor probleme de retea intalnite pana acum
- **Rezerva IP static si pentru hub** (acum pe WiFi, la fel de vulnerabil la schimbari DHCP) - alternativ, adauga un al doilea cablu ethernet si revino la conexiune cablata pe ambele masini
- **Configureaza Music Assistant cu o sursa de muzica** (Spotify / YouTube Music / fisiere locale - decizie inca nefacuta)

## Music Assistant - pasi in lant, dupa alegerea sursei

- Activeaza "Home Assistant Player Provider" in Music Assistant
- Expune entitatile relevante catre Assist (media player + satelit)
- Instaleaza blueprint-ul comunitar "Music Assistant Voice Support" (suporta si formulari libere)

## Curatenie / verificari ramase

- Verifica/sterge add-on-urile HAOS redundante ramase din arhitectura veche (Wyoming Satellite, openWakeWord) si integrarea veche "Wyoming Protocol"
- Corecteaza zona (Area) a satelitului daca "Bedroom" nu e camera reala

## Pe termen mai lung

- **Integrarea Claude (API) in pipeline** - inca neinceput. E pasul care transforma sistemul din "asistent cu comenzi locale" in AI conversational real
- Decide licenta finala pentru vocea Piper (Sanda vs. mihai) inainte de orice folosire in afara prototipului personal
- Model Whisper mai rapid / hardware accelerat, pentru latenta mai mica (~1-2 secunde acum)
- Aplicatia de contorizare tokenuri (`token-tracker/`) - inca neinceputa
