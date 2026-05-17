# Övningar till kapitel 7: Kamera som följer spelaren

## Övning 1: Grundläggande kameraföljning

Skapa scriptet `CameraFollow`, lägg det på `Main Camera` och koppla spelarens GameObject till `target`.

Testa att spela banan från vänster till höger.

### Kontrollfrågor

- Syns spelaren hela tiden?
- Rör sig kameran mjukt?
- Behåller kameran ett rimligt Z-värde?

## Övning 2: Offset-test

Testa minst tre olika offset-inställningar:

```csharp
new Vector3(0f, 1f, -10f)
new Vector3(2f, 1f, -10f)
new Vector3(0f, 2f, -10f)
```

Skriv ner vilken inställning som fungerar bäst för din bana.

## Övning 3: Smooth speed-test

Testa `smoothSpeed` med värdena `2`, `5` och `10`.

Beskriv kort hur kameran känns vid varje värde.

## Fördjupning: Följ bara i X-led

Ändra scriptet så att kameran följer spelarens X-position men behåller sin egen Y-position.

Ledtråd:

```csharp
Vector3 desiredPosition = new Vector3(
    target.position.x + offset.x,
    transform.position.y,
    offset.z
);
```

Fundera sedan på när detta kan passa bättre än att följa både X och Y.
