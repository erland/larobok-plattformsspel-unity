# Kapitel 9: Hälsa, liv och checkpoints

## Varför detta kapitel finns

I förra kapitlet skapade vi fiender och hinder som kan upptäcka när spelaren rör vid dem. Än så länge händer bara något i Console. Det är användbart för testning, men spelaren märker ingen tydlig konsekvens i själva spelet.

I det här kapitlet gör vi farorna meningsfulla. Vi lägger till hälsa, skada, respawn och checkpoints. När spelaren träffar en fiende eller ett hinder ska hälsan minska. Om hälsan tar slut ska spelaren återvända till den senaste checkpointen.

Målet är att skapa en enkel spelstruktur: spelaren kan misslyckas, komma tillbaka och försöka igen.

## Lärandemål

Efter kapitlet ska du kunna:

- skapa ett enkelt hälsosystem för spelaren
- låta farliga objekt skada spelaren
- flytta tillbaka spelaren till en respawn-position
- skapa checkpoints som uppdaterar spelarens startpunkt
- dela upp ansvar mellan flera scripts

## Innan vi börjar

Du behöver projektet från föregående kapitel. Det ska innehålla:

- en spelare med rörelse, hopp och collider
- minst en fiende eller ett hinder med `DamageZone`
- taggen `Player` på spelarobjektet
- en bana där spelaren kan falla, hoppa och träffa faror

Vi kommer att återanvända `DamageZone`, men ändra vad den gör. I stället för att bara skriva ett meddelande ska den anropa ett script på spelaren.

## Huvudförklaring

### Vad betyder hälsa i ett enkelt plattformsspel?

Hälsa är ett tal som beskriver hur många träffar spelaren tål innan något händer.

I Skogssprånget börjar vi enkelt:

- spelaren har ett maximalt antal hälsopoäng
- varje farligt objekt gör en viss mängd skada
- när hälsan når noll flyttas spelaren tillbaka till en checkpoint
- hälsan återställs efter respawn

Det här är inte den enda möjliga modellen. Vissa spel använder liv, hjärtan, energimätare eller direkt död. Vi börjar med hälsopoäng eftersom det är tydligt och lätt att bygga vidare på.

### Nytt script: PlayerHealth

Skapa ett nytt C#-script som heter `PlayerHealth` och koppla det till spelarobjektet.

```csharp
using UnityEngine;

public class PlayerHealth : MonoBehaviour
{
    public int maxHealth = 3;

    private int currentHealth;
    private Vector3 respawnPosition;

    private void Start()
    {
        currentHealth = maxHealth;
        respawnPosition = transform.position;
    }

    public void TakeDamage(int damageAmount)
    {
        currentHealth -= damageAmount;

        Debug.Log("Player health: " + currentHealth);

        if (currentHealth <= 0)
        {
            Respawn();
        }
    }

    public void SetCheckpoint(Vector3 checkpointPosition)
    {
        respawnPosition = checkpointPosition;
        Debug.Log("Checkpoint updated.");
    }

    private void Respawn()
    {
        transform.position = respawnPosition;
        currentHealth = maxHealth;

        Debug.Log("Player respawned.");
    }
}
```

Det här scriptet har tre viktiga ansvarsområden:

- hålla reda på spelarens hälsa
- veta var spelaren ska återuppstå
- återställa spelaren när hälsan tar slut

### Varför är TakeDamage public?

Metoden `TakeDamage` är markerad som `public` eftersom andra scripts ska kunna anropa den.

Det betyder att ett farligt objekt kan säga:

```csharp
playerHealth.TakeDamage(1);
```

Det farliga objektet behöver inte veta hur hälsosystemet fungerar inuti. Det behöver bara veta att spelaren kan ta skada.

Det är en viktig programmeringsidé: ett script ska helst veta så lite som möjligt om andra scripts interna detaljer.

### Uppdatera DamageZone

Öppna scriptet `DamageZone` från förra kapitlet. Byt ut innehållet mot denna version:

```csharp
using UnityEngine;

public class DamageZone : MonoBehaviour
{
    public int damageAmount = 1;

    private void OnTriggerEnter2D(Collider2D other)
    {
        if (other.CompareTag("Player"))
        {
            PlayerHealth playerHealth = other.GetComponent<PlayerHealth>();

            if (playerHealth != null)
            {
                playerHealth.TakeDamage(damageAmount);
            }
        }
    }
}
```

Nu händer flera saker i ordning:

1. `OnTriggerEnter2D` körs när något går in i triggern.
2. Scriptet kontrollerar om objektet har taggen `Player`.
3. Scriptet försöker hämta spelarens `PlayerHealth`.
4. Om `PlayerHealth` finns anropas `TakeDamage`.

`GetComponent<PlayerHealth>()` betyder: leta efter en component av typen `PlayerHealth` på samma GameObject som collider-kontakten kom från.

### Testa skada

Gör ett första test innan du bygger checkpoints:

1. Markera spelaren.
2. Kontrollera att `PlayerHealth` finns på spelaren.
3. Kontrollera att spelarens tagg är `Player`.
4. Markera en fiende eller ett hinder.
5. Kontrollera att objektet har `DamageZone`.
6. Starta spelet.
7. Rör vid hindret flera gånger.

I Console bör du se att spelarens hälsa minskar. När hälsan når noll ska spelaren flyttas tillbaka till sin startposition.

### Problem: spelaren kan ta skada för snabbt

Om spelaren står kvar i ett farligt objekt kan skada ibland upplevas som för snabb eller förvirrande. I vår första version används `OnTriggerEnter2D`, vilket bara körs när spelaren går in i triggern. Det är ett bra första steg.

Senare kan du vilja bygga ett system med tillfällig odödlighet efter skada, ofta kallat invulnerability frames eller i-frames. Vi väntar med det för att inte göra kapitlet för stort.

Just nu är målet att skapa ett begripligt grundsystem.

### Skapa en checkpoint

En checkpoint är en plats som sparar var spelaren ska återuppstå.

Skapa ett nytt GameObject i scenen:

1. Skapa ett tomt GameObject.
2. Döp det till `Checkpoint`.
3. Lägg till en `BoxCollider2D`.
4. Aktivera `Is Trigger`.
5. Placera checkpointen på banan där spelaren ska kunna passera den.

Om du vill att checkpointen ska synas kan du lägga till en sprite, till exempel en flagga eller skylt. För testning räcker det med ett tomt objekt med collider.

### Script: Checkpoint

Skapa ett nytt C#-script som heter `Checkpoint` och koppla det till checkpoint-objektet.

```csharp
using UnityEngine;

public class Checkpoint : MonoBehaviour
{
    private void OnTriggerEnter2D(Collider2D other)
    {
        if (other.CompareTag("Player"))
        {
            PlayerHealth playerHealth = other.GetComponent<PlayerHealth>();

            if (playerHealth != null)
            {
                playerHealth.SetCheckpoint(transform.position);
            }
        }
    }
}
```

När spelaren går genom checkpointen hämtas spelarens `PlayerHealth`, och checkpointens position skickas till `SetCheckpoint`.

Nu vet spelaren var den ska komma tillbaka nästa gång hälsan tar slut.

### Testa checkpointen

Bygg en kort testsekvens:

1. Placera spelaren vid början av banan.
2. Placera en checkpoint efter ett enkelt hopp.
3. Placera ett farligt hinder efter checkpointen.
4. Starta spelet.
5. Gå först till checkpointen.
6. Rör sedan vid hindret tills hälsan når noll.

Om allt fungerar ska spelaren återvända till checkpointen, inte till den ursprungliga startpositionen.

### Respawn och Rigidbody2D

När spelaren flyttas direkt med `transform.position` kan Rigidbody2D fortfarande ha gammal hastighet kvar. Det kan göra att spelaren fortsätter röra sig direkt efter respawn.

Vi kan lösa det genom att låta `PlayerHealth` känna till spelarens `Rigidbody2D`.

Uppdatera `PlayerHealth` till denna version:

```csharp
using UnityEngine;

public class PlayerHealth : MonoBehaviour
{
    public int maxHealth = 3;

    private int currentHealth;
    private Vector3 respawnPosition;
    private Rigidbody2D rb;

    private void Start()
    {
        currentHealth = maxHealth;
        respawnPosition = transform.position;
        rb = GetComponent<Rigidbody2D>();
    }

    public void TakeDamage(int damageAmount)
    {
        currentHealth -= damageAmount;

        Debug.Log("Player health: " + currentHealth);

        if (currentHealth <= 0)
        {
            Respawn();
        }
    }

    public void SetCheckpoint(Vector3 checkpointPosition)
    {
        respawnPosition = checkpointPosition;
        Debug.Log("Checkpoint updated.");
    }

    private void Respawn()
    {
        transform.position = respawnPosition;

        if (rb != null)
        {
            rb.linearVelocity = Vector2.zero;
        }

        currentHealth = maxHealth;

        Debug.Log("Player respawned.");
    }
}
```

Raden `rb.linearVelocity = Vector2.zero;` nollställer spelarens fart. Om din Unity-version använder `velocity` i stället för `linearVelocity` kan du behöva skriva:

```csharp
rb.velocity = Vector2.zero;
```

Använd det namn som fungerar i din installerade Unity-version.

### Fallzon under banan

En vanlig plattformsspelsregel är att spelaren ska återuppstå om hen faller ner från banan. Det kan byggas med samma `DamageZone`.

Skapa en fallzon:

1. Skapa ett nytt GameObject och döp det till `FallZone`.
2. Lägg till `BoxCollider2D`.
3. Aktivera `Is Trigger`.
4. Gör collidern bred och placera den under hela banan.
5. Lägg till `DamageZone`.
6. Sätt `Damage Amount` till 3 om spelaren ska återuppstå direkt.

Eftersom spelarens `maxHealth` är 3 gör en skada på 3 att hälsan når noll direkt.

Det är återanvändning: fiender, taggar och fallzoner kan använda samma grundscript men med olika värden.

## Exempel: checkpoints i Skogssprånget

I Skogssprånget kan första banan nu få en enkel struktur:

1. Spelaren startar vid skogens början.
2. En kort trygg plattform låter spelaren testa rörelse och hopp.
3. En checkpoint placeras före en svårare fiendesektion.
4. En patrullerande fiende vaktar nästa plattform.
5. En fallzon ligger under banan.
6. Om spelaren misslyckas återvänder hen till checkpointen.

Det gör banan mer rättvis. Spelaren behöver inte börja om från början varje gång något går fel.

## Vanliga misstag

- **Inget händer när spelaren rör vid hindret.**
  - Varför det händer: spelaren saknar `PlayerHealth`, saknar taggen `Player` eller hindret saknar `DamageZone`.
  - Hur man undviker det: kontrollera först spelarobjektet och sedan hindret i Inspector.

- **Checkpointen uppdateras inte.**
  - Varför det händer: checkpointens collider är inte markerad som `Is Trigger`.
  - Hur man undviker det: markera checkpointen och kontrollera `BoxCollider2D`.

- **Spelaren återuppstår men flyger vidare.**
  - Varför det händer: Rigidbody2D har kvar hastighet från fallet eller träffen.
  - Hur man undviker det: nollställ hastigheten i `Respawn()`.

- **Spelaren återuppstår inuti ett hinder.**
  - Varför det händer: checkpointen är placerad för nära en fiende, vägg eller fallzon.
  - Hur man undviker det: placera checkpoints på tydliga, säkra ytor.

- **DamageZone försöker göra för mycket.**
  - Varför det händer: det är lockande att lägga hälsa, respawn och checkpointlogik direkt i faro-scriptet.
  - Hur man undviker det: låt `DamageZone` bara dela ut skada och låt `PlayerHealth` hantera spelaren.

## Övningar

### Övning 1: Ändra hur mycket skada faror gör

Sätt olika `Damage Amount` på olika faror:

- fiende: 1
- taggar: 1 eller 2
- fallzon: 3

Testa hur det påverkar spelets känsla. Känns det rättvist?

### Övning 2: Lägg till två checkpoints

Placera två checkpoints på banan:

1. en efter en enkel del
2. en före en svårare del

Testa att spelaren återvänder till den senaste checkpointen, inte den första.

### Övning 3: Gör respawn-platsen tydlig

Lägg till en enkel visuell markering vid checkpointen, till exempel en flagga, skylt eller ljus punkt.

Fundera på hur spelaren kan förstå att checkpointen är aktiverad även utan att läsa Console.

### Fördjupning

Lägg till en kort paus efter respawn genom att skriva ett meddelande i Console och vänta med nästa förbättring tills du vet att grundsystemet fungerar.

När du är redo kan du senare förbättra systemet med:

- blinkande spelare efter skada
- tillfällig odödlighet
- hjärtan i UI
- ljud när checkpointen aktiveras

## Snabb sammanfattning

- `PlayerHealth` håller reda på hälsa, skada och respawn-position.
- `DamageZone` delar ut skada när spelaren rör vid farliga objekt.
- `Checkpoint` uppdaterar spelarens respawn-position.
- `GetComponent<T>()` används för att hämta ett script eller en component från ett GameObject.
- Respawn blir stabilare om spelarens Rigidbody2D-hastighet nollställs.
- Samma `DamageZone` kan användas för fiender, hinder och fallzoner.

## Quiz/reflektionsfrågor

1. Varför ligger hälsologiken i `PlayerHealth` och inte direkt i `DamageZone`?
2. Vad gör `GetComponent<PlayerHealth>()`?
3. Varför behöver checkpointens collider vara en trigger?
4. Vad kan hända om spelarens hastighet inte nollställs vid respawn?
5. Hur kan checkpoints göra en bana mer rättvis?

## Nästa steg

Nu har spelet konsekvenser när spelaren misslyckas. I nästa kapitel lägger vi till belöningar: samlingsobjekt, poäng och enkel UI-feedback. Då får spelaren inte bara saker att undvika, utan också saker att sträva efter.
