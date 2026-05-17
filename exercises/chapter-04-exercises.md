# Övningar till kapitel 4: Spelarens första rörelse

## Övning 1: Hitta en bra rörelsehastighet

1. Öppna scenen där `Player` finns.
2. Markera `Player`.
3. Ändra `Move Speed` i Inspector.
4. Testa minst tre värden, till exempel `3`, `6` och `10`.

Skriv ner vilket värde du vill använda tills vidare och varför.

## Övning 2: Bygg en enkel testbana

Skapa en markyta och två väggar med `BoxCollider2D`.

Målet är att spelaren ska kunna röra sig åt vänster och höger, men stoppas av väggarna.

Kontrollera:

- att spelaren inte faller genom marken
- att spelaren inte passerar genom väggarna
- att spelaren inte börjar rotera okontrollerat

## Övning 3: Undersök input

Lägg till tillfälligt:

```csharp
Debug.Log(horizontalInput);
```

Testa spelet och observera värdena i Console.

Besvara:

1. Vilket värde visas när du trycker vänster?
2. Vilket värde visas när du trycker höger?
3. Vilket värde visas när du inte trycker någon riktning?

Ta bort `Debug.Log`-raden när du är klar.

## Reflektion

Vad är skillnaden mellan att läsa spelarens input och att faktiskt flytta spelarens Rigidbody2D?
