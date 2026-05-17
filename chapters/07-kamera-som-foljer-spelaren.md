# Kapitel 7: Kamera som följer spelaren

## Varför detta kapitel finns

I förra kapitlet byggde vi en första bana för **Skogssprånget**. Spelaren kan nu röra sig, hoppa och landa på plattformar. Men om banan är större än det som syns i spelvyn uppstår ett nytt problem: kameran står stilla medan spelaren springer iväg.

I ett plattformsspel är kameran en del av spelkänslan. Den avgör vad spelaren ser, hur långt framåt spelaren kan planera och om hopp känns rättvisa eller frustrerande.

I det här kapitlet gör vi en enkel kamera som följer spelaren. Vi börjar med en egen liten lösning i C#, eftersom den visar principen tydligt. Senare kan du byta till mer avancerade kameraverktyg om projektet behöver det.

Vi introducerar tre huvudbegrepp:

- kamera
- följmål, eller *follow target*
- spelvy

## Lärandemål

Efter kapitlet ska läsaren kunna:

- förstå vilken roll kameran har i ett 2D-plattformsspel
- skilja mellan scenens arbetsvy och spelets kameravy
- skapa ett script som låter kameran följa spelaren
- justera kamerans position med en offset
- undvika vanliga problem som skakig kamera eller felaktigt djupvärde

## Innan vi börjar

Vi fortsätter från kapitel 6. Projektet bör nu ha:

- en spelare med rörelse och hopp
- en enkel bana med mark och plattformar
- en `Main Camera` i scenen
- ett spelbart område som är bredare än kamerans första vy

Om du ännu bara har en mycket kort bana kan du förlänga den innan du börjar. Lägg gärna till några plattformar åt höger så att spelaren kan springa utanför den ursprungliga kamerabilden.

## Huvudförklaring

### Kameran visar spelets värld

När du arbetar i Unity ser du ofta två viktiga vyer:

- **Scene-vyn**, där du bygger och flyttar objekt
- **Game-vyn**, där du ser vad kameran faktiskt visar i spelet

Scene-vyn är din arbetsyta. Du kan zooma, panorera och titta runt utan att det påverkar spelet. Game-vyn visar det spelaren kommer att se när spelet körs.

Det är därför möjligt att se hela banan i Scene-vyn, men bara en liten del i Game-vyn. Det är normalt. Spelaren ser inte världen direkt. Spelaren ser världen genom kameran.

### Main Camera är också ett GameObject

Kameran i Unity är ett GameObject med en Camera-component. Den har därför också en Transform med position, rotation och skala.

I ett 2D-spel tittar kameran vanligtvis mot scenen från ett fast djup. Det betyder att kamerans X- och Y-position kan följa spelaren, medan Z-positionen oftast ska ligga kvar på ett negativt värde, till exempel `-10`.

Det här är viktigt. Om kamerans Z-värde råkar bli samma som spelarens kan kameran hamna inne i scenen och visa fel eller ingenting alls.

### Vad betyder follow target?

Ett **follow target** är det objekt som kameran ska följa. I vårt fall är det spelarens Transform.

Kameran behöver inte veta allt om spelaren. Den behöver inte veta om spelaren hoppar, tar skada eller samlar mynt. Den behöver bara veta var spelaren befinner sig.

Det gör lösningen enkel:

1. kameran läser spelarens position
2. kameran skapar en ny kameraposition
3. kameran behåller sitt eget Z-värde
4. kameran flyttas dit

### Direkt följning eller mjuk följning

Det finns två enkla sätt att låta kameran följa spelaren:

- **direkt följning:** kameran hoppar direkt till spelarens position
- **mjuk följning:** kameran rör sig gradvis mot spelarens position

Direkt följning är lätt att förstå men kan kännas stel. Mjuk följning känns ofta bättre, men introducerar en liten fördröjning. I det här kapitlet använder vi mjuk följning med `Vector3.Lerp`.

`Lerp` är en vanlig förkortning för *linear interpolation*. Du behöver inte kunna den matematiska detaljen ännu. I praktiken betyder det här: rör dig en bit från nuvarande position mot målpositionen.

## Exempel

### Steg 1: Placera kameran ungefär rätt

Börja med att markera `Main Camera` i Hierarchy.

Sätt ungefär dessa värden i Transform:

- Position X: `0`
- Position Y: `1`
- Position Z: `-10`
- Rotation X: `0`
- Rotation Y: `0`
- Rotation Z: `0`

Välj sedan Game-vyn och starta spelet. Kontrollera att spelaren syns.

Om spelaren är för liten eller för stor kan du justera kamerans `Size` i Camera-componenten. För ett 2D-spel med orthographic camera styr `Size` hur mycket av världen som syns vertikalt. Ett större värde visar mer av banan, men gör allt mindre.

### Steg 2: Skapa scriptet CameraFollow

Skapa ett nytt C#-script i projektet och döp det till `CameraFollow`.

Lägg scriptet på `Main Camera`.

Skriv sedan följande kod:

```csharp
using UnityEngine;

public class CameraFollow : MonoBehaviour
{
    public Transform target;
    public Vector3 offset = new Vector3(0f, 1f, -10f);
    public float smoothSpeed = 5f;

    private void LateUpdate()
    {
        if (target == null)
        {
            return;
        }

        Vector3 desiredPosition = target.position + offset;
        Vector3 smoothedPosition = Vector3.Lerp(
            transform.position,
            desiredPosition,
            smoothSpeed * Time.deltaTime
        );

        transform.position = smoothedPosition;
    }
}
```

Det här scriptet gör tre viktiga saker:

- `target` säger vilket objekt kameran ska följa
- `offset` säger hur kameran ska ligga i förhållande till spelaren
- `smoothSpeed` styr hur snabbt kameran rör sig mot målpositionen

Vi använder `LateUpdate()` i stället för `Update()`. Tanken är att spelaren först hinner flytta sig under bildrutan. Därefter flyttar kameran efter. Det minskar risken för att kameran känns en aning ur fas med spelaren.

### Steg 3: Koppla spelaren som target

Markera `Main Camera` i Hierarchy.

I Inspector ser du nu komponenten `CameraFollow`.

Dra spelarens GameObject från Hierarchy till fältet `Target`.

Starta spelet och spring åt höger. Kameran ska nu följa spelaren.

Om kameran inte rör sig är det vanligaste felet att `Target` inte är ifyllt.

### Steg 4: Justera offset

Offset-värdet bestämmer var spelaren hamnar i bilden.

Med detta värde ligger kameran lite ovanför spelaren:

```csharp
public Vector3 offset = new Vector3(0f, 1f, -10f);
```

Prova till exempel:

- `new Vector3(0f, 0f, -10f)` för att centrera spelaren
- `new Vector3(2f, 1f, -10f)` för att visa mer av världen framför spelaren åt höger
- `new Vector3(0f, 2f, -10f)` för att visa mer ovanför spelaren

Var försiktig med Z-värdet. I ett vanligt 2D-projekt ska det ofta fortsätta vara `-10`.

### Steg 5: Justera mjukheten

`smoothSpeed` styr hur snabbt kameran följer efter.

Ett lågt värde ger en långsammare och mjukare kamera. Ett högt värde gör kameran snabbare och mer direkt.

Testa till exempel:

- `2` för långsam följning
- `5` för en balanserad start
- `10` för snabb följning

För **Skogssprånget** rekommenderar jag att börja med `5`. Det är tillräckligt mjukt för att kameran inte ska kännas helt låst, men tillräckligt snabbt för att spelaren inte ska springa ifrån bilden.

## Vanliga misstag

- **Misstag: Kameran visar bara blå bakgrund eller ingenting.**
  - Varför det händer: Kamerans Z-position har hamnat fel, eller kameran tittar inte mot spelvärlden.
  - Hur man undviker det: Kontrollera att kamerans Z-värde är negativt, till exempel `-10`, och att rotationen är nollställd.

- **Misstag: Kameran följer inte spelaren.**
  - Varför det händer: `target` är inte ifyllt i Inspector.
  - Hur man undviker det: Dra spelarens GameObject till Target-fältet på `CameraFollow`.

- **Misstag: Kameran känns skakig.**
  - Varför det händer: Spelaren flyttas i fysiksteg medan kameran uppdateras vid fel tidpunkt, eller flera scripts flyttar kameran samtidigt.
  - Hur man undviker det: Använd `LateUpdate()` för kameran och kontrollera att bara ett script styr kamerans position.

- **Misstag: Spelaren hamnar exakt i mitten men man ser inte vad som kommer.**
  - Varför det händer: Kameran följer spelarens position utan framåtblick.
  - Hur man undviker det: Använd en liten X-offset eller bygg banan så att kamerans utsnitt ger spelaren tid att reagera.

## Övningar

### Övning 1: Få kameran att följa spelaren

Skapa `CameraFollow`, koppla det till `Main Camera` och dra in spelaren som `target`.

Testa sedan att springa genom hela banan från kapitel 6.

Kontrollera:

- att spelaren syns hela tiden
- att kameran inte tappar bort spelaren
- att kamerans Z-position inte ändras till fel värde

### Övning 2: Hitta en bra offset

Testa minst tre olika offset-värden.

Skriv ner vilket värde som känns bäst och varför. Fundera särskilt på:

- ser spelaren tillräckligt mycket framför sig?
- känns hopp lättare eller svårare?
- syns plattformar i tid?

### Övning 3: Jämför snabb och långsam kamera

Testa `smoothSpeed` med värdena `2`, `5` och `10`.

Skriv en kort kommentar för varje värde:

- Hur känns kontrollen?
- Tappar du överblicken?
- Känns kameran lugn eller stressig?

### Fördjupning

Just nu följer kameran spelaren i både X- och Y-led. I vissa plattformsspel följer kameran bara spelaren horisontellt, eller följer Y-led långsammare för att undvika att kameran hoppar upp och ner vid varje litet hopp.

Skapa en variant där kameran följer spelarens X-position men behåller sin egen Y-position.

Ledtråd: skapa `desiredPosition` genom att kombinera spelarens X-värde med kamerans nuvarande Y-värde.

## Snabb sammanfattning

- Game-vyn visar vad kameran ser, inte hela scenen.
- `Main Camera` är ett GameObject med position och komponenter.
- Ett follow target är objektet kameran följer.
- `LateUpdate()` passar bra för kamerarörelse eftersom spelaren hinner flyttas först.
- `offset` styr var spelaren hamnar i bilden.
- `smoothSpeed` styr hur snabbt kameran rör sig mot spelaren.
- Kamerans Z-värde måste hållas rätt i ett 2D-spel.

## Quiz/reflektionsfrågor

1. Vad är skillnaden mellan Scene-vyn och Game-vyn?
2. Varför behöver kameran oftast behålla ett negativt Z-värde i ett 2D-spel?
3. Vad används `target` till i `CameraFollow`?
4. Varför kan `LateUpdate()` vara bättre än `Update()` för en följande kamera?
5. Hur påverkar offset spelupplevelsen i ett plattformsspel?

## Nästa steg

Nu kan spelaren röra sig genom en bana medan kameran följer med. Spelet börjar kännas mer sammanhängande.

I nästa kapitel lägger vi till fiender och hinder. Då behöver vi fundera på hur objekt kan röra sig utan direkt spelarinput, hur kollisioner kan orsaka konsekvenser och hur vi kan skapa enkla faror som gör banan mer intressant.
