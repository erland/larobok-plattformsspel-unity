# Kapitel 11: Animationer och spelkänsla

## Varför detta kapitel finns

Nu har Skogssprånget flera viktiga spelregler: spelaren kan röra sig, hoppa, ta skada, återvända till checkpoints och samla objekt. Spelet fungerar, men det kan fortfarande kännas stelt. När spelaren trycker på en tangent händer något i koden, men spelaren behöver också se och höra att något händer.

Det är här animationer och spelkänsla kommer in. Animationer gör att spelaren verkar levande. Spelkänsla handlar om små detaljer som gör att spelet känns tydligt, responsivt och roligt att spela.

I det här kapitlet lägger vi till enkla animationer för spelarens idle-, gå- och hopplägen. Vi tittar också på enkel feedback för samlingsobjekt och skada.

## Lärandemål

Efter kapitlet ska du kunna:

- förklara vad Animator och animation states används till
- skapa enkla animationer för en spelarkaraktär
- styra animationer från ett C#-script med parametrar
- skilja mellan spelmekanik och spelkänsla
- lägga till enkel visuell eller auditiv feedback utan att göra projektet rörigt

## Innan vi börjar

Du behöver projektet från föregående kapitel. Det ska innehålla:

- en spelare som kan röra sig och hoppa
- ett fungerande `PlayerController`-script
- en bana med samlingsobjekt
- ett poängsystem med UI
- grundläggande kollisions- och triggerlogik

I det här kapitlet behöver du också några enkla sprites för spelaren. Det räcker med mycket enkla placeholder-bilder. Om du saknar färdiga sprites kan du använda färgade rutor eller några enkla bilder från samma spritepaket som tidigare.

## Huvudförklaring

### Vad är spelkänsla?

Spelkänsla är hur spelet upplevs när spelaren styr det. Två spel kan ha nästan samma kod men kännas helt olika.

Ett plattformsspel känns ofta bättre när:

- spelaren får tydlig visuell feedback vid rörelse
- hoppet syns i animationen
- objekt ger återkoppling när de samlas in
- skada märks direkt
- ljud och små effekter förstärker viktiga händelser

I det här kapitlet börjar vi enkelt. Vi gör inte ett avancerat animationssystem. Målet är att lära dig grunderna så att du kan bygga vidare senare.

### Animation, Animator och animation state

I Unity finns flera begrepp som lätt blandas ihop:

- **Animation clip** är själva animationen, till exempel en gå-animation.
- **Animator** är komponenten som spelar upp animationer på ett GameObject.
- **Animator Controller** är en fil som beskriver vilka animationer som finns och hur de byter mellan varandra.
- **Animation state** är ett läge i Animator Controller, till exempel `Idle`, `Run` eller `Jump`.

Du kan tänka på Animator Controller som en liten karta över spelarens visuella lägen. När spelaren står stilla är den i `Idle`. När spelaren rör sig är den i `Run`. När spelaren är i luften är den i `Jump`.

### Förbered spelarens Animator

Markera spelarobjektet i Hierarchy.

Kontrollera att spelaren har:

- `Sprite Renderer`
- `Rigidbody2D`
- `Collider2D`
- `PlayerController`

Lägg sedan till komponenten `Animator`.

Skapa en ny mapp i Project-vyn:

```text
Animations
```

I mappen skapar du en Animator Controller och döper den till:

```text
PlayerAnimatorController
```

Dra filen till fältet **Controller** i spelarens Animator-komponent.

### Skapa tre enkla animation clips

Skapa tre animationer:

```text
Player_Idle
Player_Run
Player_Jump
```

För en första version kan animationerna vara mycket enkla:

- `Player_Idle`: en stillastående sprite
- `Player_Run`: två eller flera sprites som växlar
- `Player_Jump`: en sprite som visar att spelaren hoppar eller faller

Om du inte har flera sprites ännu kan du ändå skapa strukturen. Du kan byta ut bilderna senare när du har riktig grafik.

Öppna Animator-fönstret. Du bör se tre states: `Player_Idle`, `Player_Run` och `Player_Jump`.

Gör `Player_Idle` till standardläge om det inte redan är det.

### Animator-parametrar

För att koden ska kunna styra animationerna behöver Animator Controller parametrar.

Skapa två parametrar:

| Namn | Typ | Syfte |
|---|---|---|
| `Speed` | Float | Hur snabbt spelaren rör sig horisontellt. |
| `IsGrounded` | Bool | Om spelaren står på marken. |

Vi använder `Speed` för att växla mellan idle och run. Vi använder `IsGrounded` för att veta om spelaren ska visa hoppanimation.

### Övergångar mellan animationer

Skapa transitions mellan dina states.

Från `Player_Idle` till `Player_Run`:

- villkor: `Speed` större än `0.1`

Från `Player_Run` till `Player_Idle`:

- villkor: `Speed` mindre än `0.1`

Från `Any State` till `Player_Jump`:

- villkor: `IsGrounded` är `false`

Från `Player_Jump` till `Player_Idle`:

- villkor: `IsGrounded` är `true`
- och `Speed` mindre än `0.1`

Från `Player_Jump` till `Player_Run`:

- villkor: `IsGrounded` är `true`
- och `Speed` större än `0.1`

För en nybörjarvänlig känsla kan du börja med korta övergångar. Om animationerna känns sega kan du minska transition duration i Inspector.

### Uppdatera PlayerController

Nu behöver `PlayerController` skicka information till Animator.

Lägg till ett fält för Animator:

```csharp
private Animator animator;
```

I `Awake` eller `Start` hämtar du komponenten:

```csharp
private void Awake()
{
    rb = GetComponent<Rigidbody2D>();
    animator = GetComponent<Animator>();
}
```

Om ditt script redan har `Awake`, lägg bara till raden som hämtar `Animator`.

Sedan kan du uppdatera Animator i `Update` eller efter att rörelsen har beräknats.

Exempel:

```csharp
private void UpdateAnimation()
{
    float speed = Mathf.Abs(rb.linearVelocity.x);

    animator.SetFloat("Speed", speed);
    animator.SetBool("IsGrounded", isGrounded);
}
```

Anropa metoden från `Update`:

```csharp
private void Update()
{
    horizontalInput = Input.GetAxisRaw("Horizontal");

    if (Input.GetButtonDown("Jump") && isGrounded)
    {
        Jump();
    }

    UpdateAnimation();
}
```

Om din Unity-version eller ditt projekt använder `rb.velocity` i stället för `rb.linearVelocity`, fortsätt med samma variant som du redan använt i tidigare kapitel. Blanda inte båda i samma script.

### Vänd spelaren åt rätt håll

En viktig del av spelkänslan är att spelaren tittar åt rätt håll.

Ett enkelt sätt är att vända spelarens sprite genom `SpriteRenderer.flipX`.

Lägg till ett fält:

```csharp
private SpriteRenderer spriteRenderer;
```

Hämta komponenten i `Awake`:

```csharp
spriteRenderer = GetComponent<SpriteRenderer>();
```

Skapa sedan en metod:

```csharp
private void UpdateFacingDirection()
{
    if (horizontalInput > 0)
    {
        spriteRenderer.flipX = false;
    }
    else if (horizontalInput < 0)
    {
        spriteRenderer.flipX = true;
    }
}
```

Anropa den från `Update`:

```csharp
UpdateFacingDirection();
```

Det här gör att spelaren tittar åt höger när input är positiv och åt vänster när input är negativ.

### Samlad kodskiss

Din `PlayerController` kan se olika ut beroende på hur du följt tidigare kapitel. Här är en förenklad kodskiss som visar de nya delarna. Anpassa den till ditt befintliga script.

```csharp
using UnityEngine;

public class PlayerController : MonoBehaviour
{
    [SerializeField] private float moveSpeed = 6f;
    [SerializeField] private float jumpForce = 12f;

    private Rigidbody2D rb;
    private Animator animator;
    private SpriteRenderer spriteRenderer;

    private float horizontalInput;
    private bool isGrounded;

    private void Awake()
    {
        rb = GetComponent<Rigidbody2D>();
        animator = GetComponent<Animator>();
        spriteRenderer = GetComponent<SpriteRenderer>();
    }

    private void Update()
    {
        horizontalInput = Input.GetAxisRaw("Horizontal");

        if (Input.GetButtonDown("Jump") && isGrounded)
        {
            Jump();
        }

        UpdateFacingDirection();
        UpdateAnimation();
    }

    private void FixedUpdate()
    {
        rb.linearVelocity = new Vector2(horizontalInput * moveSpeed, rb.linearVelocity.y);
    }

    private void Jump()
    {
        rb.linearVelocity = new Vector2(rb.linearVelocity.x, jumpForce);
        isGrounded = false;
    }

    private void UpdateFacingDirection()
    {
        if (horizontalInput > 0)
        {
            spriteRenderer.flipX = false;
        }
        else if (horizontalInput < 0)
        {
            spriteRenderer.flipX = true;
        }
    }

    private void UpdateAnimation()
    {
        float speed = Mathf.Abs(rb.linearVelocity.x);

        animator.SetFloat("Speed", speed);
        animator.SetBool("IsGrounded", isGrounded);
    }

    private void OnCollisionEnter2D(Collision2D collision)
    {
        if (collision.gameObject.CompareTag("Ground"))
        {
            isGrounded = true;
        }
    }

    private void OnCollisionExit2D(Collision2D collision)
    {
        if (collision.gameObject.CompareTag("Ground"))
        {
            isGrounded = false;
        }
    }
}
```

Viktigt: om ditt tidigare `PlayerController` redan använder en mer exakt markkontroll med ground check ska du behålla den. Ersätt inte en fungerande lösning med den enklare kollisionskontrollen ovan. Kodskissen visar bara hur Animator kopplas in.

## Exempel: feedback när spelaren samlar objekt

I kapitel 10 försvann samlingsobjektet när spelaren plockade upp det. Det fungerar, men det känns bättre om spelaren får tydligare återkoppling.

Det finns flera enkla sätt att förbättra feedback:

- spela upp ett ljud
- visa en liten partikeleffekt
- låta objektet blinka innan det försvinner
- visa en kort poängtext
- göra en liten studs- eller skalningsanimation

Vi börjar med en enkel ljudeffekt.

### Lägg till AudioSource

Skapa ett ljudobjekt i scenen:

1. Skapa ett tomt GameObject.
2. Döp det till `AudioManager`.
3. Lägg till komponenten `AudioSource`.
4. Stäng av **Play On Awake**.
5. Lägg in en kort ljudeffekt i fältet **AudioClip**.

Skapa sedan ett script:

```csharp
using UnityEngine;

public class SimpleAudioPlayer : MonoBehaviour
{
    public static SimpleAudioPlayer Instance { get; private set; }

    [SerializeField] private AudioSource audioSource;
    [SerializeField] private AudioClip collectSound;

    private void Awake()
    {
        Instance = this;
    }

    public void PlayCollectSound()
    {
        if (audioSource != null && collectSound != null)
        {
            audioSource.PlayOneShot(collectSound);
        }
    }
}
```

Koppla `AudioSource` och ljudklippet i Inspector.

I `Collectible` kan du sedan lägga till:

```csharp
SimpleAudioPlayer.Instance.PlayCollectSound();
```

Placera raden när spelaren faktiskt samlar objektet, innan objektet förstörs.

En möjlig version:

```csharp
private void OnTriggerEnter2D(Collider2D other)
{
    if (!other.CompareTag("Player"))
    {
        return;
    }

    ScoreManager.Instance.AddScore(scoreValue);
    SimpleAudioPlayer.Instance.PlayCollectSound();

    Destroy(gameObject);
}
```

Det här är fortfarande enkelt, men spelet känns direkt mer responsivt.

## Vanliga misstag

- **Misstag: Animationsparametern heter nästan rätt.**
  - Varför det händer: Koden använder till exempel `"IsGrounded"` men Animator-parametern heter `"isGrounded"`.
  - Hur man undviker det: Kopiera namnet exakt och var konsekvent med stora och små bokstäver.

- **Misstag: Spelaren fastnar i hoppanimationen.**
  - Varför det händer: `IsGrounded` sätts aldrig tillbaka till `true`.
  - Hur man undviker det: Testa markkontrollen med `Debug.Log` och kontrollera att markobjekten har rätt tag eller layer.

- **Misstag: Animationerna känns sega.**
  - Varför det händer: Transitions i Animator har för lång övergångstid.
  - Hur man undviker det: Minska transition duration och testa igen i Play Mode.

- **Misstag: Koden kraschar när ljudet spelas.**
  - Varför det händer: `SimpleAudioPlayer.Instance`, `AudioSource` eller `AudioClip` saknas.
  - Hur man undviker det: Kontrollera att `AudioManager` finns i scenen och att fälten är kopplade i Inspector.

- **Misstag: För mycket polish för tidigt.**
  - Varför det händer: Det är roligt att putsa animationer och ljud, men spelets grundregler behöver fortfarande fungera.
  - Hur man undviker det: Lägg till små förbättringar som hjälper testningen, men spara större grafiskt arbete till senare.

## Övningar

### Övning 1: Skapa spelarens tre grundanimationer

Skapa eller koppla tre animationer:

1. idle
2. run
3. jump

Testa att spelaren växlar mellan dem när du står stilla, springer och hoppar.

### Övning 2: Justera spelkänslan

Testa att ändra:

- övergångstiden mellan animationer
- spelarens `moveSpeed`
- spelarens `jumpForce`
- hur snabbt kameran följer spelaren

Skriv ner vilka ändringar som gjorde spelet tydligare eller roligare.

### Övning 3: Lägg till feedback på samlingsobjekt

Välj en förbättring:

- ljudeffekt när ett objekt samlas in
- liten partikeleffekt
- kort blinkning
- annan sprite precis innan objektet försvinner

Börja med en enkel version. Målet är tydligare feedback, inte perfekt grafik.

### Fördjupning

Skapa en bool-parameter som heter `IsHurt` och använd den för att spela en kort skadereaktion när spelaren tar skada.

Du behöver då fundera på:

- när `IsHurt` ska sättas till `true`
- när den ska återställas till `false`
- om animationen ska kunna avbryta andra animationer

## Snabb sammanfattning

- Animationer gör att spelaren får tydliga visuella lägen.
- Animator Controller styr vilka animation states som finns och hur de byter mellan varandra.
- C#-kod kan styra Animator med parametrar som `Speed` och `IsGrounded`.
- Spelkänsla handlar om respons, tydlighet och återkoppling.
- Små förbättringar som ljud, blinkningar och bättre animationer kan göra stor skillnad.

## Quiz/reflektionsfrågor

1. Vad är skillnaden mellan ett animation clip och en Animator Controller?
2. Varför är det viktigt att parameternamn i kod och Animator matchar exakt?
3. När passar det att använda en bool-parameter i Animator?
4. Vad betyder spelkänsla i ett plattformsspel?
5. Nämn två sätt att ge feedback när spelaren samlar ett objekt.

## Nästa steg

Nu känns prototypen mer levande. I nästa kapitel knyter vi ihop spelets struktur med menyer, nivåbyte och en tydligare spel-loop. Då får Skogssprånget en början, ett slut och ett sätt att starta om när spelaren vill försöka igen.
