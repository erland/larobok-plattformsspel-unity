# Övningar till kapitel 10: Samla objekt och visa poäng

## Övning 1: Skapa samlingsobjekt

1. Skapa ett GameObject som heter `Berry`.
2. Lägg till en sprite.
3. Lägg till en 2D-collider.
4. Markera `Is Trigger`.
5. Koppla scriptet `Collectible`.
6. Testa att spelaren kan samla objektet.

## Övning 2: Visa poäng på skärmen

Skapa ett `ScoreManager`-objekt och en UI-text.

Kontrollera att:

- texten börjar på `Poäng: 0`
- poängen ökar när spelaren samlar ett objekt
- inga fel visas i Console

## Övning 3: Skapa risk och belöning

Placera ett samlingsobjekt nära en fiende eller ett hinder.

Fundera på:

- Är belöningen värd risken?
- Behöver objektet ge fler poäng?
- Är placeringen rättvis för spelaren?

## Reflektionsfrågor

1. Vilket ansvar har `Collectible`?
2. Vilket ansvar har `ScoreManager`?
3. Varför är det praktiskt att kunna ändra `Points` i Inspector?
4. Vad skulle hända om `ScoreText` inte kopplas till `ScoreManager`?
