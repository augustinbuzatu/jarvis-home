# Cum lucram la proiectul asta

Ghid scurt pentru cine ajuta la proiect, ca sa ramanem sincronizati fara sa ne stricam munca unii altora.

## Branch-uri

Nu se lucreaza direct pe `main`. Pentru orice modificare:

```bash
git checkout -b feature/nume-scurt-descriptiv
```

Exemple: `feature/token-tracker-ui`, `fix/wakeword-sensibilitate`, `docs/setup-raspberry-pi`.

Prefixe folosite:
- `feature/` - functionalitate noua
- `fix/` - reparare bug
- `docs/` - documentatie
- `chore/` - configurari, curatenie, mentenanta

## Commit-uri

Mesaje scurte, la subiect, in engleza sau romana (consistent in cadrul aceluiasi commit):

```
feat: adauga comanda locala pentru termostat
fix: corecteaza pragul de detectie wake word
docs: actualizeaza ghidul de instalare pentru Pi
```

Evitati commit-uri de tipul "fix", "update", "asdf" - ingreuneaza cautarea ulterioara in istoric.

## Pull Requests

Cand o functionalitate e gata:

1. Push pe branch-ul propriu: `git push origin feature/nume-branch`
2. Deschide un Pull Request catre `main` pe GitHub
3. Descrie pe scurt ce face PR-ul si, daca e cazul, cum a fost testat
4. Asteapta cel putin o aprobare de la un coleg inainte de merge

## Issues

Task-urile ramase (ex: polish token-tracker, documentatie lipsa) se tin ca **Issues** pe GitHub, nu doar in capul cuiva. Cand incepi sa lucrezi la unul, asigneaza-l pe numele tau.

## Secrete si date sensibile

Niciodata nu se face commit la:
- `secrets.yaml` sau orice fisier cu chei API, parole, tokenuri
- Fisiere `.env`
- Baza de date interna Home Assistant (`.storage/`, `*.db`)

Daca ai nevoie de o valoare noua in `secrets.yaml`, adaug-o si in `secrets.yaml.example` cu valoare goala, ca ceilalti sa stie ce trebuie sa completeze.

Daca nu esti sigur daca un fisier e sigur de urcat, intreaba inainte de commit, nu dupa.
