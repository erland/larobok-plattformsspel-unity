# Övningar till kapitel 5: Hopp, gravitation och markkontroll

## Övning 1: Hitta en bra hoppstyrka

Testa minst tre olika värden för `jumpForce`.

Förslag:

- `7`
- `10`
- `13`

Skriv kort:

- Vilket värde kändes bäst?
- Var hoppet för lågt, för högt eller lagom?
- Behöver banan anpassas efter hoppet?

## Övning 2: Jämför gravitation

Ändra `Gravity Scale` på spelarens `Rigidbody2D`.

Testa:

- `1`
- `3`
- `5`

Besvara:

1. När känns spelaren mest responsiv?
2. När blir spelaren svävande?
3. När blir spelaren för tung?

## Övning 3: Kontrollera GroundCheck

Flytta `GroundCheck` till tre positioner:

1. för högt upp i spelaren
2. precis vid fötterna
3. lite för långt under spelaren

Testa hoppet i varje läge och skriv vad som händer.

## Övning 4: Felsök ett hoppfel

Skapa medvetet ett fel genom att ta bort `&& isGrounded` från hoppvillkoret.

Testa spelet.

Besvara:

- Vad händer?
- Varför händer det?
- Hur återställer du korrekt beteende?

## Fördjupning: Visa markstatus tillfälligt

Lägg till en tillfällig loggning:

```csharp
Debug.Log(isGrounded);
```

Testa spelet och observera när värdet blir `true` och `false`.

Ta bort loggningen när du är klar.


## Extra övning: gör hoppet mer förlåtande

Lägg till `coyoteTime` och `jumpBufferTime` i ditt spelar-script.

Testa tre olika värden:

- `0.05`
- `0.12`
- `0.20`

Skriv kort vad som händer med spelkänslan. Vilket värde känns mest rättvist utan att göra hoppet för lätt?
