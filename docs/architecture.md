# Arhitectura sistemului (stadiu curent)

Sistem hibrid, cu doua componente fizice separate care comunica prin retea.

## Hub central - Laptop (HAOS)

Ruleaza Home Assistant OS, cu urmatoarele add-on-uri/integrari active:

- **Whisper** (speech-to-text, romana) - functional
- **Piper** (text-to-speech, romana) - functional, voce **Sanda medium** (vezi nota de licenta mai jos)
- **Assist pipeline "Asistent RO"** - functional, limba pipeline-ului setata explicit pe "Romanian" (nu codul "ro_RO" - vezi `troubleshooting.md`)
- **Music Assistant** - instalat, dar fara sursa de muzica configurata inca

Conectare la retea: WiFi (schimbat de pe ethernet - vezi decizia in `progress-log.md`, sesiunea 4).

## Satelit - Raspberry Pi

Are microfon (Superlux E205U) si boxe (Serioux, jack 3.5mm) conectate. Ruleaza **Linux Voice Assistant (LVA)**, ca serviciu systemd nativ pe Raspberry Pi OS Lite (nu Home Assistant OS - motivul e in `troubleshooting.md`).

- Wake word: `okay_nabu` (motor microWakeWord, integrat direct in LVA)
- Conectare la hub: prin protocol **ESPHome**, port `6053` (nu Wyoming Protocol - solutia initiala, abandonata)
- Suporta Media Player nativ, spre deosebire de wyoming-satellite (solutia initiala)
- Retea: ethernet (ramas cablat, spre deosebire de hub)

## Fluxul unei comenzi

1. Microfonul de pe Pi prinde wake word-ul (`okay_nabu`)
2. Audio-ul e trimis catre hub prin retea (ESPHome/LVA)
3. Whisper (pe hub) transforma audio-ul in text
4. Home Assistant Assist proceseaza comanda:
   - comanda simpla (ex. "aprinde lumina") - procesata local, prin motorul de intentii
   - comanda complexa - **planificat, neinceput inca** (vezi `roadmap.md`) - ar urma sa mearga catre Claude prin API
5. Piper genereaza raspunsul vocal (romana, voce Sanda)
6. Raspunsul e trimis inapoi la Pi si redat prin boxe

Latenta actuala end-to-end: ~1-2 secunde (normal pentru procesare CPU locala, fara hardware dedicat de accelerare).

## Licenta voce Sanda - de retinut

Vocea Piper folosita (`eduardem/piper-tts-romanian`, varianta Sanda medium) are licenta **CC-BY-NC-4.0 - doar necomercial**. Varianta de fallback, cu licenta permisiva (MIT), e vocea oficiala "mihai".
