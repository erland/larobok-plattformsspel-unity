# Övningar till kapitel 14: Bygg, testa och paketera prototypen

## Övning 1: Pre-build-kontroll

Gå igenom projektet och kontrollera att:

- startmenyn fungerar
- rätt scen startar först
- spelaren kan röra sig och hoppa
- kameran följer spelaren
- game over eller restart fungerar
- inga uppenbara testobjekt finns kvar
- Console inte fylls av återkommande fel

Skriv ned tre saker som behöver fixas innan du delar spelet.

## Övning 2: Testplan

Skapa minst åtta testfall.

Använd formatet:

| Test | Vad testaren gör | Förväntat resultat |
|---|---|---|
| | | |

## Övning 3: Skapa en build

Gör en build och starta den utanför Unity Editor.

Anteckna:

- fungerade startflödet?
- fungerade UI?
- fungerade ljud?
- kändes rörelsen likadan som i editorn?
- uppstod något fel som du inte sett tidigare?

## Övning 4: README till testare

Skriv en kort `README.txt` med:

- titel och version
- startinstruktion
- kontroller
- vad testaren ska fokusera på
- hur testaren ska rapportera buggar

## Fördjupning: Versionera prototypen

Skapa en commit och en tagg för din första spelbara version:

```bash
git add .
git commit -m "Create first playable prototype"
git tag v0.1
```

Skriv också en kort release-notering för versionen.
