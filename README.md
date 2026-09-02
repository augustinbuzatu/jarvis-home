# Smart Home Assistant (RO) - Boxa AI Conversationala

Sistem propriu de asistent vocal pentru casa, gandit ca alternativa la Google Home / Amazon Alexa, dar cu suport nativ pentru limba romana si conversatii complexe printr-un model AI (Claude), nu doar comenzi fixe.

Proiect in dezvoltare - vine cu commit-uri succesive pe masura ce se adauga functionalitati noi.

## Ce face

- Controleaza dispozitive din casa prin comenzi vocale in romana (lumini, prize, termostat etc.)
- Raspunde la intrebari si cereri complexe printr-o conversatie reala cu Claude, nu doar comenzi predefinite
- Recunoaste un wake word local, fara sa asculte constant catre cloud
- Text-to-speech si speech-to-text ruleaza local (Piper si Whisper), fara dependenta de servicii externe pentru partea de voce

Exemple concrete de comenzi si automatizari sunt in [`examples/`](./examples).

## Arhitectura, pe scurt

Sistemul are doua componente fizice care comunica intre ele:

1. **Laptop - server Home Assistant (HAOS)**
   Ruleaza Home Assistant OS, cu Piper (text-to-speech) si Whisper (speech-to-text) instalate local. Aici e configurata si integrarea cu Anthropic, care trimite cererile complexe catre Claude prin API.

2. **Raspberry Pi - satelit**
   Are boxe si un microfon conectate, ruleaza Linux si face recunoasterea wake word-ului (openWakeWord). Practic e "urechea si gura" fizica a sistemului, in timp ce toata logica sta pe laptop.

Fluxul unei comenzi: microfonul de pe Pi prinde wake word-ul -> audio-ul e trimis catre laptop -> Whisper il transforma in text -> Home Assistant decide daca e o comanda locala (ex: aprinde lumina) sau o cerere complexa care merge la Claude prin API -> raspunsul e transformat inapoi in voce de Piper -> redat pe boxele de la Pi.

Detalii complete in [`docs/architecture.md`](./docs/architecture.md).

## Structura proiectului

```
├── docs/                  # arhitectura, ghid de instalare, roadmap
├── examples/              # exemple de comenzi, conversatii si automatizari
├── home-assistant/        # configuratia HAOS (automatizari, scripturi, integrari)
├── voice-pipeline/        # configurare Piper (TTS) si Whisper (STT)
├── rpi-satellite/         # tot ce ruleaza pe Raspberry Pi (scripturi, servicii, wake word)
└── token-tracker/         # aplicatie separata pentru contorizarea tokenurilor folosite (in lucru)
```

## Status / ce mai urmeaza

- [x] HAOS + integrare Anthropic functionale
- [x] Pi ca satelit, cu wake word local
- [x] Comenzi locale pentru dispozitive
- [ ] Aplicatie de contorizare tokenuri (`token-tracker/`)
- [ ] Polish general si documentatie completa

Detalii in [`docs/roadmap.md`](./docs/roadmap.md).

## Instalare

Ghid pas cu pas in [`docs/setup.md`](./docs/setup.md).

**Important:** proiectul foloseste un fisier `secrets.yaml` pentru cheia API Anthropic si alte credentiale, care **nu este inclus in repo**. Vezi `home-assistant/secrets.yaml.example` pentru ce trebuie completat local.

## Echipa

Proiect deschis colaborarii - vezi [`CONTRIBUTING.md`](./CONTRIBUTING.md) pentru cum se lucreaza pe branch-uri si Pull Requests.
