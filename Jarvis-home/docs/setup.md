# Ghid de instalare

Presupune ca ai deja: un laptop disponibil ca hub, un Raspberry Pi cu microfon si boxe conectate, acces la reteaua locala.

## 1. Hub central (laptop) - Home Assistant OS

Instaleaza Home Assistant OS pe laptop, urmand [ghidul oficial](https://www.home-assistant.io/installation/).

Dupa instalare, adauga urmatoarele add-on-uri din Settings -> Add-ons:
- **Whisper** (speech-to-text) - seteaza limba pe romana
- **Piper** (text-to-speech) - seteaza limba pe romana

Creeaza un Assist pipeline nou (Settings -> Voice assistants -> Add assistant), cu Whisper si Piper ca STT/TTS. **Important:** seteaza limba pipeline-ului explicit pe "Romanian" (textul, nu codul "ro_RO") - altfel unele voci custom nu apar in lista de selectie (vezi `troubleshooting.md`).

## 2. (Optional) Voce Piper alternativa - Sanda

Vocea oficiala "mihai" e licentiata MIT si merge din prima, fara pasi suplimentari. Daca vrei vocea Sanda (calitate mai buna pe romana, dar licenta **CC-BY-NC-4.0, doar necomercial**):

Acceseaza consola fizica a HAOS (ecran + tastatura conectate direct la laptop), la promptul `ha >` scrie `login` pentru shell complet. Navigheaza la folderul Piper (`/mnt/data/supervisor/share/piper`) si descarca:

```bash
curl -L -o ro_RO-sanda-medium.onnx https://huggingface.co/eduardem/piper-tts-romanian/resolve/main/voices/sanda/ro_RO-sanda-medium.onnx
curl -L -o ro_RO-sanda-medium.onnx.json https://huggingface.co/eduardem/piper-tts-romanian/resolve/main/voices/sanda/ro_RO-sanda-medium.onnx.json
```

Apoi: Restart pe add-on-ul Piper + Reload pe integrarea Wyoming Protocol, apoi selecteaza vocea din Settings -> Voice assistants -> pipeline-ul tau -> Text-to-speech -> Voice.

## 3. Retea - configurare WiFi pe hub (daca nu ai cablu ethernet disponibil)

Din consola host HAOS (acces ca la pasul 2):

```bash
ha network info
```

(verifica numele interfetei WiFi - difera dupa hardware, nu presupune ca e `wlan0`)

```bash
ha network update <nume-interfata> --ipv4-method auto --wifi-auth wpa-psk --wifi-mode infrastructure --wifi-ssid "SSID-UL_TAU" --wifi-psk "PAROLA_TA"
```

Apoi, in interfata web: Settings -> System -> Network -> "Home Assistant URL" -> dezactiveaza switch-ul "Automatic" -> seteaza manual adresa locala reala a hub-ului (format `http://192.168.X.X:8123`). Fara acest pas, comenzile vocale "asculta" dar raspunsul TTS nu se aude deloc (vezi `troubleshooting.md`).

**Recomandat:** rezerva IP static pentru hub in router, ca sa nu mai fie nevoie de acest pas dupa fiecare schimbare de retea.

## 4. Satelit (Raspberry Pi) - Linux Voice Assistant

Instaleaza Raspberry Pi OS Lite pe Pi (nu Home Assistant OS - satelitul are nevoie de acces direct la hardware-ul audio, vezi `troubleshooting.md`).

Cloneaza si instaleaza [Linux Voice Assistant (LVA)](https://github.com/OHF-Voice/linux-voice-assistant) intr-un folder dedicat (ex. `~/linux-voice-assistant`), intr-un virtual environment Python.

Creeaza serviciul systemd, `/etc/systemd/system/linux-voice-assistant.service`:

```ini
[Unit]
Description=Linux-Voice-Assistant
After=network-online.target
Wants=network-online.target

[Service]
Type=simple
User=<user-ul-tau>
WorkingDirectory=/home/<user-ul-tau>/linux-voice-assistant
Environment=PATH=/home/<user-ul-tau>/linux-voice-assistant/.venv/bin:/usr/bin:/bin
Environment=CLIENT_NAME="numele-satelitului"
Environment=PULSE_SERVER="/run/user/1000/pulse/native"
Environment=XDG_RUNTIME_DIR="/run/user/1000"
Environment=PULSE_COOKIE="/home/<user-ul-tau>/linux-voice-assistant/tmp_pulse_cookie"
Environment=PREFERENCES_FILE="/home/<user-ul-tau>/linux-voice-assistant/preferences.json"
ExecStart=/home/<user-ul-tau>/linux-voice-assistant/docker-entrypoint.sh
Restart=always
RestartSec=5

[Install]
WantedBy=multi-user.target
```

Activeaza si porneste serviciul:

```bash
sudo systemctl enable linux-voice-assistant
sudo systemctl start linux-voice-assistant
```

Verifica ca ruleaza: `sudo systemctl status linux-voice-assistant`. Log live: `sudo journalctl -u linux-voice-assistant -f`.

## 5. Verifica iesirea audio (PipeWire)

Daca nu se aude nimic prin boxe, dar log-urile arata "Playing", verifica sink-ul audio implicit:

```bash
wpctl status
```

Identifica boxele reale (nu interfata audio a microfonului USB, care poate aparea gresit ca implicita) si seteaza-le explicit:

```bash
wpctl set-default <ID_SINK>
sudo systemctl restart linux-voice-assistant
```

## 6. Conecteaza satelitul la Home Assistant

Settings -> Devices & Services -> Add integration -> **ESPHome** (nu Wyoming Protocol) -> IP-ul Pi-ului, port `6053`.

Home Assistant creeaza automat un device cu ~15 entitati (Assist satellite, Media Player, Mic controls etc.). Verifica dupa creare:
- Zona (Area) setata corect (poate aparea gresit implicit, ex. "Bedroom")
- Pipeline-ul "Asistent RO" selectat pe entitatea Assist satellite

## 7. Retea - satelit

Daca Pi-ul are si WiFi si ethernet active simultan (poate cauza confuzie de IP), dezactiveaza WiFi explicit:

```bash
sudo rfkill block wifi
```

**Recomandat:** rezerva IP static pentru Pi in router (vezi `roadmap.md` - inca nefacut, cauza principala a mai multor probleme de retea intalnite pana acum).

## Ce urmeaza

Pasii de mai sus acopera comenzile locale (control dispozitive). Integrarea Claude (API), pentru comenzi conversationale complexe, e planificata dar inca neinceputa - vezi `roadmap.md`.
