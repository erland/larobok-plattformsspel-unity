# Övningar till kapitel 3: C# för Unity: grunderna du behöver

## Övning 1: Skapa ett eget testscript

Skapa scriptet `GameSettings` och koppla det till ett GameObject i en testscen.

Krav:

- `public int startLives = 3;`
- `public float gravityScale = 2.5f;`
- `public string levelName = "Forest Start";`
- värdena skrivs ut i Console när scenen startar

## Övning 2: Flytta kod till en metod

Skapa metoden `PrintSettings()` och flytta utskrifterna dit.

`Start()` ska bara anropa `PrintSettings()`.

## Övning 3: Träna på felmeddelanden

Ta bort ett semikolon, läs felmeddelandet i Console och rätta felet.

Skriv kort:

- vilken fil felet fanns i
- vilken rad Unity pekade på
- vad du gjorde för att lösa felet

## Fördjupning: CoinCounter

Skapa scriptet `CoinCounter`.

Det ska ha en publik `int coins`, metoden `AddCoin()` och tre anrop till `AddCoin()` från `Start()`.

Målet är att Console ska visa att antalet coins ökar steg för steg.
