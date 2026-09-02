# Idei pentru mai tarziu

Idei notate pe parcurs, neimplementate inca, pastrate ca sa nu se piarda.

- **Arhitectura de productie (dincolo de prototip):** un hub central per gospodarie (Raspberry Pi 5 sau mini-PC, ruland Whisper/Piper/HA) + cate o boxa-satelit ieftina per camera, ideal pe baza de ESP32-S3 + firmware ESPHome - mult mai ieftin si mai fiabil decat un Raspberry Pi intreg per camera
- **Multiple wake word-uri / pipeline-uri pe acelasi satelit** (feature disponibil in Home Assistant 2025.10+) - ar permite separarea unui pipeline local (comenzi simple, gratuit) de unul catre Claude (comenzi complexe), fiecare cu propriul cuvant de trezire, pe acelasi dispozitiv fizic
- **Media Player nativ pe satelit deschide usa spre multi-room audio** - mai multe satelite inseamna mai multe tinte de redare, gestionate central prin Music Assistant
- **Data disk separat pentru HAOS** - posibilitatea de a muta datele pe un disc extern USB, pastrand cardul SD doar pentru boot (fiabilitate mai buna decat boot complet de pe USB)
- Un al doilea cablu ethernet ar elimina complet compromisul WiFi pe hub
- Accesul la consola fizica HAOS (`login` -> bash host) e o unealta foarte utila pentru orice depanare viitoare care nu tine de add-on-uri
