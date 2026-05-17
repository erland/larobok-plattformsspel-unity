# Kapitel 4: Spelarens första rörelse

## Varför detta kapitel finns

Nu har du grunden som behövs för att börja göra Skogssprånget spelbart. Du har skapat ett Unity-projekt, lärt känna arbetsytan och skrivit enkla C#-scripts. I det här kapitlet använder vi den kunskapen till något konkret: spelaren ska kunna röra sig åt vänster och höger.

Rörelse är ett bra första spelmoment eftersom det binder ihop flera viktiga delar av Unity:

- ett GameObject som representerar spelaren
- en Component som ger objektet fysik
- ett script som läser input
- en variabel som styr hastigheten

Målet är inte att skapa perfekt plattformskänsla direkt. Målet är att få en kontrollerbar spelare och förstå varför koden ser ut som den gör.

## Lärandemål

Efter kapitlet ska du kunna:

- skapa ett enkelt Player-objekt för testning
- lägga till `Rigidbody2D` och `BoxCollider2D`
- läsa horisontell input från tangentbordet
- flytta spelaren åt vänster och höger med C#
- förstå skillnaden mellan input, hastighet och fysisk rörelse
- felsöka vanliga problem när spelaren inte rör sig

## Innan vi börjar

I förra kapitlet använde du begreppen klass, metod och variabel. De kommer tillbaka här, men i ett mer praktiskt sammanhang.

Kom ihåg:

- En **klass** är behållaren för scriptets kod.
- En **variabel** lagrar ett värde, till exempel spelarens hastighet.
- `Update()` körs varje bildruta och passar bra för att läsa spelarens input.
- Ett script kan kopplas till ett GameObject som en Component.

I det här kapitlet introducerar vi tre nya huvudbegrepp:

- **input**: spelarens knapptryckningar eller styrsignaler
- **Rigidbody2D**: en Component som låter Unitys 2D-fysik påverka objektet
- **hastighet**: hur snabbt något rör sig i en viss riktning

## Huvudförklaring

### Steg 1: Skapa en enkel testscen

Börja i en enkel scen. Om du redan har en testscen från tidigare kapitel kan du använda den. Annars kan du skapa en ny scen och spara den som `Level01`.

Skapa först marken:

1. Högerklicka i Hierarchy.
2. Skapa ett 2D Object, till exempel en Square-sprite.
3. Döp objektet till `Ground`.
4. Sätt Scale till ungefär `X = 12`, `Y = 1`, `Z = 1`.
5. Flytta objektet nedåt, till exempel till position `Y = -3`.
6. Lägg till en `BoxCollider2D` om objektet inte redan har en collider.

Skapa sedan spelaren:

1. Skapa en ny Square-sprite.
2. Döp objektet till `Player`.
3. Sätt Scale till ungefär `X = 1`, `Y = 1.5`, `Z = 1`.
4. Placera spelaren ovanför marken, till exempel `X = 0`, `Y = -1`.

Det spelar ingen roll om grafiken är enkel. I början är fyrkanter perfekta. De gör det lätt att se om rörelsen fungerar.

### Steg 2: Ge spelaren fysik

Markera `Player` i Hierarchy och lägg till två Components:

- `Rigidbody2D`
- `BoxCollider2D`

`BoxCollider2D` gör att spelaren kan kollidera med marken.

`Rigidbody2D` gör att spelaren kan påverkas av Unitys 2D-fysik, till exempel gravitation. När du trycker på Play bör spelaren falla ned och landa på marken. Om spelaren faller genom marken saknar antingen spelaren eller marken en korrekt collider.

Titta på spelarens `Rigidbody2D`. Du bör hitta inställningen **Gravity Scale**. Låt den vara på standardvärdet tills vidare. Vi arbetar med hopp och gravitation mer noggrant i nästa kapitel.

### Steg 3: Skapa scriptet PlayerMovement

Skapa en mapp som heter `Scripts` om du inte redan har en. Skapa sedan ett nytt C#-script som heter `PlayerMovement`.

Koppla scriptet till `Player`.

Öppna scriptet och ersätt innehållet med detta:

```csharp
using UnityEngine;

public class PlayerMovement : MonoBehaviour
{
    public float moveSpeed = 6f;

    private Rigidbody2D rb;
    private float horizontalInput;

    void Start()
    {
        rb = GetComponent<Rigidbody2D>();
    }

    void Update()
    {
        horizontalInput = Input.GetAxisRaw("Horizontal");
    }

    void FixedUpdate()
    {
        rb.linearVelocity = new Vector2(horizontalInput * moveSpeed, rb.linearVelocity.y);
    }
}
```

Det här scriptet gör tre saker:

1. Hämtar spelarens `Rigidbody2D`.
2. Läser om spelaren trycker vänster eller höger.
3. Sätter spelarens horisontella hastighet.

Om din Unity-version visar fel på `linearVelocity` kan du använda `velocity` i stället:

```csharp
rb.velocity = new Vector2(horizontalInput * moveSpeed, rb.velocity.y);
```

Unitys API kan förändras mellan versioner. Därför är det viktigt att kontrollera felmeddelanden och anpassa kodexempel efter den Unity-version du använder.

### Steg 4: Förstå variablerna

Scriptet börjar med tre variabler:

```csharp
public float moveSpeed = 6f;

private Rigidbody2D rb;
private float horizontalInput;
```

`moveSpeed` är spelarens rörelsehastighet. Eftersom den är `public` syns den i Inspector. Det betyder att du kan ändra värdet utan att skriva om koden.

`rb` är en variabel som ska hålla reda på spelarens `Rigidbody2D`. Vi använder den för att prata med fysikkomponenten.

`horizontalInput` lagrar spelarens input i sidled. När spelaren trycker vänster blir värdet normalt `-1`. När spelaren trycker höger blir det `1`. När spelaren inte trycker åt något håll blir det `0`.

### Steg 5: Hämta spelarens Rigidbody2D

I `Start()` finns denna rad:

```csharp
rb = GetComponent<Rigidbody2D>();
```

`GetComponent<Rigidbody2D>()` betyder ungefär: “hämta den Rigidbody2D som sitter på samma GameObject som det här scriptet”.

Det fungerar eftersom `PlayerMovement` och `Rigidbody2D` båda sitter på `Player`.

Om du glömmer att lägga till `Rigidbody2D` på spelaren får scriptet inget att hämta. Då kan du få ett fel när spelet körs. Det är ett vanligt och nyttigt fel: koden förväntar sig en Component som inte finns.

### Steg 6: Läsa input i Update

I `Update()` finns denna rad:

```csharp
horizontalInput = Input.GetAxisRaw("Horizontal");
```

`Input.GetAxisRaw("Horizontal")` läser Unitys förinställda horisontella input. I ett standardprojekt betyder det oftast:

- vänsterpil eller A ger `-1`
- högerpil eller D ger `1`
- ingen knapp ger `0`

Vi läser input i `Update()` eftersom `Update()` körs varje bildruta. Det gör att spelet snabbt märker när spelaren trycker eller släpper en tangent.

### Steg 7: Flytta med FixedUpdate

I scriptet finns också `FixedUpdate()`:

```csharp
void FixedUpdate()
{
    rb.linearVelocity = new Vector2(horizontalInput * moveSpeed, rb.linearVelocity.y);
}
```

`FixedUpdate()` används ofta när vi arbetar med fysik. Den körs i Unitys fysiksteg, inte nödvändigtvis exakt en gång per bildruta.

Raden skapar en ny hastighet för spelarens Rigidbody2D.

```csharp
new Vector2(horizontalInput * moveSpeed, rb.linearVelocity.y)
```

En `Vector2` innehåller två värden:

- X: rörelse i sidled
- Y: rörelse uppåt eller nedåt

Vi ändrar X-värdet så att spelaren kan gå vänster och höger. Vi behåller Y-värdet från spelarens nuvarande hastighet. Det är viktigt, eftersom gravitationen fortfarande behöver kunna dra spelaren nedåt.

Om vi satte Y-värdet till `0` varje gång skulle vi råka stoppa fallrörelsen. Det skulle kännas konstigt och göra det svårare att lägga till hopp senare.

## Exempel: första spelbara kontrollen i Skogssprånget

När allt är kopplat bör du kunna trycka Play och styra den enkla spelarfiguren.

Testa detta:

1. Tryck Play.
2. Håll inne högerpil eller D.
3. Släpp knappen.
4. Håll inne vänsterpil eller A.
5. Ändra `Move Speed` i Inspector.
6. Testa igen.

Du har nu skapat den första riktiga spelmekaniken i Skogssprånget: spelaren kan röra sig i sidled.

Det är fortfarande mycket som saknas. Spelaren kan inte hoppa, kameran följer inte med och banan är bara en testyta. Men grunden är viktig: ett GameObject med fysik, ett script som läser input och en hastighet som styr rörelsen.

## Vanliga misstag

### Spelaren rör sig inte

Kontrollera först att `PlayerMovement` sitter på `Player`.

Kontrollera sedan att `Player` har en `Rigidbody2D`. Scriptet försöker hämta den komponenten i `Start()`.

Kontrollera också Console. Om Unity visar ett rött felmeddelande är det ofta mer användbart än det först ser ut. Läs särskilt vilken rad felet pekar på.

### Spelaren faller genom marken

Då saknas oftast en collider.

Kontrollera att:

- `Player` har `BoxCollider2D`
- `Ground` har `BoxCollider2D`
- spelaren och marken inte ligger på konstiga positioner
- ingen collider är avstängd

### Spelaren välter eller roterar

En fyrkantig spelare kan ibland rotera om fysiken påverkar den. Markera `Player`, gå till `Rigidbody2D` och leta efter **Constraints**. Frys rotation på Z-axeln om spelaren börjar tippa.

Det här är vanligt i 2D-spel. Spelaren ska kunna röra sig och falla, men figuren ska oftast inte rotera som en låda.

### Koden fungerar inte på grund av linearVelocity

Olika Unity-versioner kan visa olika rekommenderade egenskaper för `Rigidbody2D`. Om `linearVelocity` ger fel kan du byta till `velocity`.

Använd då samma princip överallt i scriptet:

```csharp
rb.velocity = new Vector2(horizontalInput * moveSpeed, rb.velocity.y);
```

Blanda inte `linearVelocity` och `velocity` i samma script om din Unity-version inte stöder båda tydligt.

### Rörelsen känns för snabb eller för långsam

Ändra `Move Speed` i Inspector. Testa till exempel:

- `3` för långsam rörelse
- `6` för normal teströrelse
- `10` för snabb rörelse

Det finns inget perfekt värde än. Spelkänsla utvecklas genom testning.

## Övningar

### Övning 1: Justera rörelsehastigheten

Ändra `Move Speed` i Inspector och testa minst tre olika värden.

Skriv ner:

- vilket värde som kändes för långsamt
- vilket värde som kändes för snabbt
- vilket värde du vill använda tills vidare

### Övning 2: Lägg till en enkel vägg

Skapa två smala fyrkanter på vänster och höger sida av testbanan. Ge dem `BoxCollider2D` och testa om spelaren stoppas av dem.

Fundera på varför detta fungerar utan att du skriver extra kod.

### Övning 3: Skriv ut inputvärdet

Lägg till denna rad i `Update()`:

```csharp
Debug.Log(horizontalInput);
```

Tryck Play och titta i Console medan du trycker vänster och höger.

Ta sedan bort raden igen när du har förstått vad som händer. För många `Debug.Log` i `Update()` kan göra Console svår att läsa.

### Fördjupning: Separera input och rörelse

Titta på scriptet och fundera på varför input läses i `Update()` medan rörelsen sker i `FixedUpdate()`.

Du behöver inte kunna hela svaret än. Det viktiga är att börja se att olika Unity-metoder passar olika typer av arbete.

## Snabb sammanfattning

- `Input.GetAxisRaw("Horizontal")` läser vänster- och högerrörelse.
- `Rigidbody2D` låter Unitys 2D-fysik påverka spelaren.
- `moveSpeed` styr hur snabbt spelaren rör sig.
- `FixedUpdate()` används ofta för fysikrelaterad rörelse.
- När vi ändrar X-hastigheten bör vi behålla Y-hastigheten så att gravitationen fortsätter fungera.
- En enkel fyrkantig spelare räcker bra när rörelsen testas.

## Quiz/reflektionsfrågor

1. Varför behöver `Player` både `Rigidbody2D` och `BoxCollider2D`?
2. Vad betyder värdet `-1`, `0` eller `1` från `Input.GetAxisRaw("Horizontal")`?
3. Varför sparar vi spelarens `Rigidbody2D` i variabeln `rb`?
4. Vad kan hända om vi sätter Y-hastigheten till `0` varje gång spelaren rör sig?
5. Varför är det praktiskt att `moveSpeed` är `public`?

## Nästa steg

Nu kan spelaren röra sig åt vänster och höger. I nästa kapitel bygger vi vidare på samma script och lägger till hopp. Då får du arbeta med gravitation, markkontroll och en vanlig utmaning i plattformsspel: spelaren ska bara kunna hoppa när den faktiskt står på marken.
