# Exemple de comenzi vocale

Comenzi reale (sau tipice) date sistemului, ca sa se vada ce poate sa faca. Completati tabelele pe masura ce testati.

## Comenzi locale (fara Claude API)

Executate direct de Home Assistant, fara sa treaca prin AI - raspuns rapid, fara cost de API.

| Comanda (RO) | Ce se intampla |
|---|---|
| "Aprinde lumina din living" | HA seteaza direct entitatea `light.living` pe ON |
| "Seteaza termostatul la 21 de grade" | HA trimite comanda catre `climate.termostat` |
| _(adauga aici)_ | |

## Comenzi complexe (prin Claude API)

Cereri care nu sunt comenzi fixe - au nevoie de intelegere de context sau de un raspuns conversational.

| Comanda (RO) | Ce se intampla |
|---|---|
| "Ce temperatura ar trebui sa pun seara, ca sa nu se raceasca prea tare pana dimineata?" | Cererea ajunge la Claude prin integrarea Anthropic, care raspunde tinand cont de context |
| _(adauga aici)_ | |

## Automatizari

Actiuni declansate automat, nu direct de o comanda vocala.

| Declansator | Actiune |
|---|---|
| Telefonul paraseste zona casei | Se sting luminile si se seteaza alarma |
| _(adauga aici)_ | |

## Media

_(adauga aici poze sau un GIF scurt cu sistemul in functiune, in acest folder)_
