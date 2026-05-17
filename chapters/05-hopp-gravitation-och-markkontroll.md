# Kapitel 5: Hopp, gravitation och markkontroll

## Varför detta kapitel finns

I förra kapitlet fick spelaren sin första horisontella rörelse. Figuren kunde gå åt vänster och höger, men den kunde ännu inte bete sig som en riktig plattformsspelsfigur. Ett plattformsspel behöver nästan alltid hopp, tyngd och en tydlig känsla av när spelaren står på marken.

I det här kapitlet bygger vi vidare på `PlayerMovement` och lägger till tre centrala delar:

- hopp
- gravitation
- markkontroll, eller *ground check*

Målet är inte att skapa perfekt spelkänsla direkt. Målet är att skapa en stabil grund som senare kan förbättras med animationer, ljud, effekter och mer avancerad kontroll.

## Lärandemål

Efter kapitlet ska läsaren kunna:

- förklara varför hopp i Unity ofta görs genom att ändra Y-hastigheten på en `Rigidbody2D`
- lägga till enkel hoppinput i ett spelarscript
- använda en markkontroll för att hindra spelaren från att hoppa i luften
- justera gravitation och hoppstyrka för att påverka spelkänslan
- känna igen vanliga buggar som dubbelhopp, svävande rörelse och för svag markkontroll

## Innan vi börjar

Vi fortsätter från spelaren i kapitel 4. Där skapade vi ett `Player`-objekt med:

- `Rigidbody2D`
- `BoxCollider2D`
- scriptet `PlayerMovement`
- horisontell rörelse via `Update()` och `FixedUpdate()`

Det viktigaste att komma ihåg är detta: när vi ändrar spelarens hastighet måste vi vara försiktiga så att vi inte råkar skriva över rörelse i fel riktning.

I kapitel 4 satte vi X-hastigheten, men bevarade Y-hastigheten:

```csharp
body.linearVelocity = new Vector2(moveInput * moveSpeed, body.linearVelocity.y);
```

Det är samma tanke vi använder nu, men tvärtom. När spelaren hoppar vill vi ändra Y-hastigheten, men inte förstöra X-rörelsen.

## Huvudförklaring

### Vad ett hopp egentligen är

I ett enkelt 2D-spel är ett hopp inte en animation och inte en förflyttning direkt uppåt med `Transform`. Ett hopp är oftast att spelaren får en hastighet uppåt. Sedan drar gravitationen ner spelaren igen.

Det betyder att vi kan tänka så här:

1. Spelaren trycker på hoppknappen.
2. Spelet kontrollerar om spelaren står på marken.
3. Om spelaren står på marken får `Rigidbody2D` en positiv Y-hastighet.
4. Unitys fysiksystem låter gravitationen dra spelaren ner igen.

Det här ger en enkel men stabil hoppmodell.

### Lägg till hoppstyrka

Öppna scriptet `PlayerMovement`. Vi börjar med att lägga till en ny inställning för hoppstyrka:

```csharp
using UnityEngine;

public class PlayerMovement : MonoBehaviour
{
    public float moveSpeed = 6f;
    public float jumpForce = 10f;

    private Rigidbody2D body;
    private float moveInput;

    private void Start()
    {
        body = GetComponent<Rigidbody2D>();
    }

    private void Update()
    {
        moveInput = Input.GetAxisRaw("Horizontal");

        if (Input.GetButtonDown("Jump"))
        {
            Jump();
        }
    }

    private void FixedUpdate()
    {
        body.linearVelocity = new Vector2(moveInput * moveSpeed, body.linearVelocity.y);
    }

    private void Jump()
    {
        body.linearVelocity = new Vector2(body.linearVelocity.x, jumpForce);
    }
}
```

Spara scriptet och gå tillbaka till Unity. Testa spelet.

Om allt fungerar kan spelaren hoppa. Men det finns ett problem: spelaren kan troligen hoppa igen och igen i luften. Det gör att figuren kan flyga.

Det är roligt i några sekunder, men det är inte den kontroll vi vill ha.

### Varför markkontroll behövs

För att hindra flygande spelare behöver spelet veta om spelaren står på marken. Det kallas ofta *ground check*.

En markkontroll är en liten kontrollpunkt nära spelarens fötter. Den frågar ungefär:

“Finns det mark precis här under spelaren?”

Om svaret är ja får spelaren hoppa. Om svaret är nej ska hoppet ignoreras.

### Skapa ett GroundCheck-objekt

Gör så här i Unity:

1. Markera `Player` i `Hierarchy`.
2. Högerklicka på `Player`.
3. Välj `Create Empty`.
4. Döp det nya objektet till `GroundCheck`.
5. Placera `GroundCheck` vid spelarens fötter, strax under mitten av spelaren.

Eftersom `GroundCheck` ligger som barn till `Player` följer det med när spelaren rör sig.

### Skapa ett marklager

För att markkontrollen ska veta vad som räknas som mark behöver vi ett lager.

Gör så här:

1. Markera markobjektet eller plattformen.
2. I `Inspector`, öppna menyn `Layer`.
3. Välj `Add Layer...` om du inte redan har ett marklager.
4. Skapa ett lager som heter `Ground`.
5. Gå tillbaka till markobjektet och välj lagret `Ground`.

Nu kan scriptet fråga om `GroundCheck` överlappar något som ligger på lagret `Ground`.

### Lägg till markkontroll i scriptet

Nu uppdaterar vi `PlayerMovement`:

```csharp
using UnityEngine;

public class PlayerMovement : MonoBehaviour
{
    public float moveSpeed = 6f;
    public float jumpForce = 10f;

    public Transform groundCheck;
    public float groundCheckRadius = 0.15f;
    public LayerMask groundLayer;

    private Rigidbody2D body;
    private float moveInput;
    private bool isGrounded;

    private void Start()
    {
        body = GetComponent<Rigidbody2D>();
    }

    private void Update()
    {
        moveInput = Input.GetAxisRaw("Horizontal");

        isGrounded = Physics2D.OverlapCircle(
            groundCheck.position,
            groundCheckRadius,
            groundLayer
        );

        if (Input.GetButtonDown("Jump") && isGrounded)
        {
            Jump();
        }
    }

    private void FixedUpdate()
    {
        body.linearVelocity = new Vector2(moveInput * moveSpeed, body.linearVelocity.y);
    }

    private void Jump()
    {
        body.linearVelocity = new Vector2(body.linearVelocity.x, jumpForce);
    }
}
```

Det här scriptet introducerar tre nya fält:

```csharp
public Transform groundCheck;
public float groundCheckRadius = 0.15f;
public LayerMask groundLayer;
```

`groundCheck` är punkten vid spelarens fötter.

`groundCheckRadius` är hur stort område runt punkten som ska kontrolleras.

`groundLayer` anger vilka objekt som räknas som mark.

### Koppla fälten i Inspector

När scriptet är sparat behöver Unity veta vilka objekt och lager som ska användas.

Gör så här:

1. Markera `Player`.
2. Hitta scriptet `PlayerMovement` i `Inspector`.
3. Dra `GroundCheck` från `Hierarchy` till fältet `Ground Check`.
4. Välj `Ground` i fältet `Ground Layer`.
5. Låt `Ground Check Radius` vara `0.15` till att börja med.

Testa spelet igen.

Nu ska spelaren bara kunna hoppa när den står på marken.

## Exempel: Skogssprånget får sitt första hopp

I vårt spelprojekt **Skogssprånget** är spelaren en figur som ska ta sig fram i en skogsbana. Hittills kunde figuren bara röra sig åt vänster och höger. Nu kan den hoppa mellan enkla plattformar.

En bra första känsla kan vara:

- `Move Speed`: `6`
- `Jump Force`: `10`
- `Gravity Scale` på `Rigidbody2D`: `3`
- `Ground Check Radius`: `0.15`

Det här är inte magiska värden. De är startpunkter.

Om hoppet känns för lågt kan du höja `Jump Force`.

Om spelaren känns för svävande kan du höja `Gravity Scale`.

Om spelaren ofta inte får hoppa trots att den står på marken kan du justera placeringen på `GroundCheck` eller öka `Ground Check Radius` lite.

## Att justera gravitation

Markera `Player` och titta på `Rigidbody2D`.

Där finns inställningen `Gravity Scale`. Den styr hur starkt Unitys gravitation påverkar spelaren.

Ett värde nära `1` kan kännas långsamt och svävande. Ett högre värde, till exempel `3`, gör att spelaren faller snabbare.

För plattformsspel är det vanligt att standardgravitationen behöver justeras. En snabbare fallrörelse kan göra spelet tydligare och mer responsivt.

Testa gärna tre varianter:

| Gravity Scale | Känsla |
|---|---|
| 1 | Långsamt och svävande |
| 3 | Snabbare och tydligare |
| 5 | Tungt och mer intensivt |

Det viktiga är inte att välja rätt direkt. Det viktiga är att förstå att hoppkänsla skapas av flera värden tillsammans:

- `jumpForce`
- `moveSpeed`
- `Gravity Scale`
- spelarens storlek
- banans avstånd mellan plattformar

## Förbättra hoppkänslan: coyote time och jump buffering

När grundhoppet fungerar kan spelet fortfarande kännas lite orättvist. Två vanliga situationer är:

- spelaren trycker på hopp precis efter att figuren har lämnat kanten
- spelaren trycker på hopp precis innan figuren landar

I båda fallen tycker spelaren ofta att spelet borde förstå vad hen försökte göra. Därför använder många plattformsspel två små toleranser: *coyote time* och *jump buffering*.

### Coyote time

*Coyote time* betyder att spelaren får hoppa en mycket kort stund efter att figuren egentligen har lämnat marken. Namnet kommer från tecknade filmer där en figur kan hänga kvar i luften ett ögonblick innan den faller.

Det handlar inte om att fuska. Det handlar om att kompensera för mänsklig reaktionstid och göra spelet mer rättvist.

Lägg först till en inställning och en räknare i `PlayerMovement`:

```csharp
[SerializeField] private float coyoteTime = 0.12f;

private float coyoteTimeCounter;
```

Uppdatera sedan räknaren i `Update()`:

```csharp
if (isGrounded)
{
    coyoteTimeCounter = coyoteTime;
}
else
{
    coyoteTimeCounter -= Time.deltaTime;
}
```

När spelaren hoppar kontrollerar vi inte längre bara `isGrounded`. Vi kontrollerar om coyote-räknaren fortfarande är större än noll:

```csharp
if (Input.GetButtonDown("Jump") && coyoteTimeCounter > 0f)
{
    rb.linearVelocity = new Vector2(rb.linearVelocity.x, jumpForce);
    coyoteTimeCounter = 0f;
}
```

Poängen är enkel: om spelaren nyligen stod på marken får hoppet fortfarande räknas.

### Jump buffering

*Jump buffering* betyder att spelet minns ett hopptryck under en mycket kort stund. Om spelaren trycker på hopp precis innan figuren landar, utförs hoppet så fort figuren rör marken.

Lägg till en inställning och en räknare:

```csharp
[SerializeField] private float jumpBufferTime = 0.12f;

private float jumpBufferCounter;
```

När spelaren trycker på hopp fyller vi bufferten:

```csharp
if (Input.GetButtonDown("Jump"))
{
    jumpBufferCounter = jumpBufferTime;
}
else
{
    jumpBufferCounter -= Time.deltaTime;
}
```

Sedan låter vi hoppet ske om både hoppbufferten och coyote-tiden är aktiva:

```csharp
if (jumpBufferCounter > 0f && coyoteTimeCounter > 0f)
{
    rb.linearVelocity = new Vector2(rb.linearVelocity.x, jumpForce);

    jumpBufferCounter = 0f;
    coyoteTimeCounter = 0f;
}
```

Nu kan spelet tolka spelarens avsikt bättre. Läsaren behöver inte använda dessa förbättringar direkt i sitt projekt, men det är bra att känna igen teknikerna eftersom de är vanliga i moderna plattformsspel.

### En samlad version av hopp-logiken

En förenklad `Update()` kan nu se ut så här:

```csharp
private void Update()
{
    float moveInput = Input.GetAxisRaw("Horizontal");
    rb.linearVelocity = new Vector2(moveInput * moveSpeed, rb.linearVelocity.y);

    isGrounded = Physics2D.OverlapCircle(groundCheck.position, groundCheckRadius, groundLayer);

    if (isGrounded)
    {
        coyoteTimeCounter = coyoteTime;
    }
    else
    {
        coyoteTimeCounter -= Time.deltaTime;
    }

    if (Input.GetButtonDown("Jump"))
    {
        jumpBufferCounter = jumpBufferTime;
    }
    else
    {
        jumpBufferCounter -= Time.deltaTime;
    }

    if (jumpBufferCounter > 0f && coyoteTimeCounter > 0f)
    {
        rb.linearVelocity = new Vector2(rb.linearVelocity.x, jumpForce);

        jumpBufferCounter = 0f;
        coyoteTimeCounter = 0f;
    }
}
```

Om din Unity-version använder `velocity` i stället för `linearVelocity` byter du bara ut namnet i kodexemplet. Principen är densamma.

### Felsökning av coyote time och jump buffering

- Om spelaren kan dubbelhoppa av misstag:
  - Kontrollera att `coyoteTimeCounter` sätts till `0f` efter hoppet.
- Om hoppet sker för sent:
  - Minska `jumpBufferTime`.
- Om spelaren kan hoppa för långt efter en kant:
  - Minska `coyoteTime`.
- Om hoppet aldrig triggas:
  - Kontrollera att `isGrounded` faktiskt blir `true` när spelaren står på marken.

Ett bra startvärde för både coyote time och jump buffer är `0.08` till `0.15` sekunder. Små skillnader känns tydligt i ett plattformsspel.

## Vanliga misstag

### Spelaren kan hoppa i luften

Misstag: hoppet körs utan att kontrollera `isGrounded`.

Varför det händer: scriptet lyssnar på hoppknappen men frågar inte om spelaren står på marken.

Hur du undviker det: använd villkoret:

```csharp
if (Input.GetButtonDown("Jump") && isGrounded)
{
    Jump();
}
```

### Spelaren kan inte hoppa alls

Misstag: `GroundCheck` är inte kopplat i `Inspector`, ligger på fel plats eller `Ground Layer` är inte valt.

Varför det händer: `Physics2D.OverlapCircle` hittar ingen mark.

Hur du undviker det:

- kontrollera att `GroundCheck` ligger vid spelarens fötter
- kontrollera att markobjektet har lagret `Ground`
- kontrollera att `Ground Layer` är valt på `PlayerMovement`

### Spelaren tappar sidrörelse när den hoppar

Misstag: hoppet skriver över hela hastigheten med ett nytt värde där X blir `0`.

Exempel på felaktig kod:

```csharp
body.linearVelocity = new Vector2(0f, jumpForce);
```

Varför det händer: X-hastigheten bevaras inte.

Hur du undviker det:

```csharp
body.linearVelocity = new Vector2(body.linearVelocity.x, jumpForce);
```

### Hoppet känns för svävande

Misstag: gravitationen är för låg i förhållande till hoppstyrkan.

Varför det händer: spelaren får hög Y-hastighet men faller långsamt.

Hur du undviker det: testa att höja `Gravity Scale` på `Rigidbody2D`.

## Övningar

### Övning 1: Gör hoppet tydligare

Ändra `jumpForce` till tre olika värden, till exempel `7`, `10` och `13`.

Skriv ner vilket värde som känns bäst och varför.

### Övning 2: Justera gravitationen

Testa `Gravity Scale` med värdena `1`, `3` och `5`.

Besvara:

1. Vilket värde känns mest som ett plattformsspel?
2. När blir spelaren för tung?
3. När blir spelaren för svävande?

### Övning 3: Felsök markkontrollen

Flytta `GroundCheck` lite för högt, testa spelet och observera vad som händer. Flytta sedan tillbaka det till en bättre plats.

Syftet är att förstå att små positionsändringar kan påverka om spelet uppfattar spelaren som på marken eller inte.

### Fördjupning

Lägg till ett publikt booleskt fält som tillfälligt visar om spelaren står på marken:

```csharp
public bool showGroundedState;
```

Använd sedan `Debug.Log(isGrounded);` medan du testar. Ta bort eller kommentera bort loggningen när du är klar, annars fylls `Console` snabbt.

## Snabb sammanfattning

- Ett hopp görs ofta genom att ge `Rigidbody2D` en positiv Y-hastighet.
- X-hastigheten bör bevaras när spelaren hoppar.
- Markkontroll hindrar spelaren från att hoppa i luften.
- `Physics2D.OverlapCircle` kan användas för att kontrollera om en punkt överlappar mark.
- `Gravity Scale` påverkar hur tung och responsiv spelaren känns.
- Hoppkänsla skapas genom flera värden tillsammans, inte bara ett enda “rätt” värde.

## Quiz/reflektionsfrågor

1. Varför ska hoppet ändra Y-hastigheten men bevara X-hastigheten?
2. Vad används `GroundCheck` till?
3. Varför behöver markobjektet ligga på ett särskilt lager?
4. Vad kan vara fel om spelaren inte kan hoppa trots att den står på marken?
5. Hur påverkas spelkänslan av högre `Gravity Scale`?

## Nästa steg

Nu har spelaren både rörelse och hopp. I nästa kapitel bygger vi en enkel bana där hoppet faktiskt får betydelse. Då får **Skogssprånget** sina första plattformar, kollisionsytor och en tydligare spelmiljö.
