# Övningar till kapitel 13: Versionshantera Unity-projekt med Git och GitHub

## Övning 1: Kontrollera projektroten

Hitta Unity-projektets rotmapp. Den ska innehålla:

- `Assets/`
- `Packages/`
- `ProjectSettings/`

Skriv ned sökvägen till mappen.

## Övning 2: Skapa `.gitignore`

Skapa en `.gitignore` i projektroten och lägg till regler för Unitys tillfälliga mappar.

Kontrollera med `git status` att `Library/` inte visas som filer som ska committas.

## Övning 3: Första commit

Initiera Git och skapa första commiten.

```bash
git init
git add .
git commit -m "Initial Unity project"
```

Kontrollera historiken:

```bash
git log --oneline
```

## Övning 4: Liten ändring, tydlig commit

Gör en liten, testbar ändring i Skogssprånget. Exempel:

- ändra spelarens hastighet lite
- ändra texten på startknappen
- justera mängden poäng ett objekt ger

Testa spelet och gör en commit med ett tydligt meddelande.

## Reflektionsfrågor

1. Vilka filer ändrades när du gjorde din lilla justering?
2. Var commit-meddelandet tillräckligt tydligt för att du ska förstå det om tre månader?
3. Vilka filer eller mappar vill du aldrig råka publicera?


## Extra övning: förstå en merge conflict

Skapa en testfil i projektet, till exempel `GitConflictPractice.txt`.

1. Skriv en rad text och gör en commit.
2. Skapa en ny branch.
3. Ändra samma rad på båda brancherna.
4. Försök slå ihop brancherna.
5. Lös konflikten manuellt och gör en ny commit.

Skriv med egna ord: vad var det Git inte kunde avgöra själv?
