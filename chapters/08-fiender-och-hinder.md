# Kapitel 8: Fiender och hinder

## Varför detta kapitel finns

Hittills har Skogssprånget en spelare som kan röra sig, hoppa, landa på banan och följas av kameran. Det är en viktig grund, men ett plattformsspel behöver också motstånd. Spelaren behöver något att undvika, tajma och reagera på.

I det här kapitlet lägger vi till den första enkla fienden: en patrullerande fiende som rör sig mellan två punkter. Vi skapar också ett enkelt hinder som kan skada eller stoppa spelaren. Målet är inte att bygga ett komplett hälsosystem ännu. Det kommer i nästa kapitel. Här fokuserar vi på hur fiender och farliga objekt kan upptäcka kontakt med spelaren.

## Lärandemål

Efter kapitlet ska du kunna:

- skapa en enkel fiende som rör sig mellan två punkter
- använda collider-inställningar för att upptäcka kollisioner
- skilja på fysisk kollision och trigger-kollision
- skriva ett script som reagerar när spelaren rör vid ett farligt objekt
- placera fiender och hinder så att banan blir mer spelbar

## Innan vi börjar

Du behöver projektet från föregående kapitel. Det ska innehålla:

- en spelare med `Rigidbody2D`, collider och rörelsescript
- en enkel bana med mark och plattformar
- en kamera som följer spelaren
- minst en testscen där spelaren kan springa och hoppa

Vi kommer att fortsätta använda samma grundidé som tidigare: små scripts som gör en tydlig sak. Det gör koden lättare att förstå och lättare att felsöka.

## Huvudförklaring

### Vad är en fiende i Unity?

En fiende är oftast ett vanligt `GameObject` med några components:

- en visuell del, till exempel en sprite
- en collider så att den kan upptäcka kontakt
- ibland en `Rigidbody2D`
- ett script som bestämmer beteendet

Det viktiga är att fienden inte är magisk. Den är bara ett objekt i scenen med regler.

I vårt första exempel ska fienden inte tänka själv. Den ska patrullera mellan två punkter. När den når den ena punkten vänder den mot den andra.

### Skapa fiendeobjektet

Börja med ett enkelt testobjekt:

1. Skapa ett nytt tomt GameObject i scenen.
2. Döp det till `Enemy`.
3. Lägg till en `Sprite Renderer`.
4. Välj en enkel sprite eller använd en temporär fyrkant.
5. Lägg till `BoxCollider2D`.
6. Placera fienden på en plattform.

Om du ännu inte har egna sprites går det bra att använda enkla former. I början är tydligt beteende viktigare än snygg grafik.

### Skapa patrullpunkter

Fienden behöver två målpositioner. Ett enkelt sätt är att skapa två tomma GameObjects:

1. Skapa ett tomt GameObject och döp det till `PatrolPointA`.
2. Placera det till vänster om fienden.
3. Skapa ett till och döp det till `PatrolPointB`.
4. Placera det till höger om fienden.

Dessa punkter ska inte synas i spelet. De fungerar bara som markörer i scenen.

### Script: EnemyPatrol

Skapa ett nytt C#-script som heter `EnemyPatrol` och koppla det till `Enemy`.

```csharp
using UnityEngine;

public class EnemyPatrol : MonoBehaviour
{
    public Transform pointA;
    public Transform pointB;
    public float moveSpeed = 2f;

    private Transform currentTarget;

    private void Start()
    {
        currentTarget = pointB;
    }

    private void Update()
    {
        if (currentTarget == null)
        {
            return;
        }

        transform.position = Vector2.MoveTowards(
            transform.position,
            currentTarget.position,
            moveSpeed * Time.deltaTime
        );

        float distanceToTarget = Vector2.Distance(transform.position, currentTarget.position);

        if (distanceToTarget < 0.05f)
        {
            SwitchTarget();
        }
    }

    private void SwitchTarget()
    {
        if (currentTarget == pointA)
        {
            currentTarget = pointB;
        }
        else
        {
            currentTarget = pointA;
        }
    }
}
```

Koppla sedan punkterna i Inspector:

1. Markera `Enemy`.
2. Dra `PatrolPointA` till fältet `Point A`.
3. Dra `PatrolPointB` till fältet `Point B`.
4. Starta spelet och kontrollera att fienden rör sig mellan punkterna.

### Vad gör koden?

`pointA` och `pointB` är referenser till två `Transform`-objekt. Eftersom alla GameObjects har en `Transform` kan scriptet läsa deras positioner.

`currentTarget` håller reda på vilken punkt fienden just nu går mot.

`Vector2.MoveTowards` flyttar fienden en liten bit närmare målet varje bildruta. Uttrycket `moveSpeed * Time.deltaTime` gör att rörelsen blir jämnare och inte beroende av exakt bildhastighet.

När fienden är tillräckligt nära målet byter `SwitchTarget()` till den andra punkten.

### Fysisk kollision eller trigger?

I Unity finns två vanliga sätt att upptäcka kontakt mellan objekt:

| Typ | Vad händer? | Vanligt användningsområde |
|---|---|---|
| Vanlig collider | Objekt kan stoppa eller knuffa varandra. | Mark, väggar, plattformar |
| Trigger | Objekt upptäcker kontakt utan att stoppa rörelsen. | Mynt, checkpoints, skada, målytor |

För en fiende som ska skada spelaren vill vi ofta upptäcka kontakt utan att fienden måste fungera som en vägg. Därför är trigger ofta praktiskt.

Markera fiendens `BoxCollider2D` och aktivera `Is Trigger`.

### Skapa en tag för spelaren

För att fienden ska veta att den träffat spelaren kan vi använda en tag.

1. Markera spelarobjektet.
2. I Inspector, öppna fältet `Tag`.
3. Välj eller skapa taggen `Player`.
4. Se till att spelarobjektet faktiskt har taggen `Player`.

Taggar ska användas sparsamt, men här är de ett tydligt och enkelt sätt att känna igen spelaren.

### Script: DamageZone

Skapa ett nytt C#-script som heter `DamageZone`.

```csharp
using UnityEngine;

public class DamageZone : MonoBehaviour
{
    private void OnTriggerEnter2D(Collider2D other)
    {
        if (other.CompareTag("Player"))
        {
            Debug.Log("Player touched a dangerous object.");
        }
    }
}
```

Koppla scriptet till fienden. När spelaren rör vid fienden ska ett meddelande visas i Console.

Det här scriptet gör ännu ingen verklig skada. Det är avsiktligt. Först vill vi kontrollera att kontakten upptäcks korrekt. I nästa kapitel bygger vi vidare med hälsa, liv och respawn.

### Skapa ett stillastående hinder

Nu kan du återanvända samma `DamageZone` på andra farliga objekt.

Skapa till exempel taggar, taggiga buskar eller en grop:

1. Skapa ett nytt GameObject och döp det till `Spikes`.
2. Ge det en sprite eller enkel form.
3. Lägg till `BoxCollider2D`.
4. Aktivera `Is Trigger`.
5. Lägg till `DamageZone`.
6. Placera hindret på banan.

Nu använder både fienden och hindret samma sätt att upptäcka spelaren. Det är ett bra första exempel på återanvändbar kod.

## Exempel: en första farlig bana

I Skogssprånget kan du nu bygga en kort sekvens:

1. Spelaren börjar på en säker plattform.
2. En patrullerande fiende rör sig fram och tillbaka på nästa plattform.
3. Ett stillastående hinder ligger efter fienden.
4. Spelaren måste tajma ett hopp eller vänta in rätt ögonblick.

Det här är enkel nivådesign. Du introducerar först faran på ett tydligt sätt och låter sedan spelaren öva.

En bra första version är lätt att klara. Målet är att testa mekaniken, inte att skapa en svår bana direkt.

## Vanliga misstag

- **Fienden rör sig inte.**
  - Varför det händer: `Point A` eller `Point B` är inte kopplade i Inspector.
  - Hur man undviker det: markera fienden och kontrollera att båda fälten har objekt.

- **Fienden försvinner eller rör sig konstigt.**
  - Varför det händer: patrullpunkterna ligger långt bort, på fel höjd eller utanför banan.
  - Hur man undviker det: zooma ut i Scene-vyn och kontrollera punkternas positioner.

- **Triggern upptäcker inte spelaren.**
  - Varför det händer: spelaren saknar taggen `Player`, eller något objekt saknar rätt collider/Rigidbody2D-kombination.
  - Hur man undviker det: kontrollera spelarens tagg och att spelaren har `Rigidbody2D`.

- **Fienden blockerar spelaren som en vägg.**
  - Varför det händer: `Is Trigger` är inte aktiverat på fiendens collider.
  - Hur man undviker det: aktivera `Is Trigger` om fienden bara ska upptäcka kontakt.

- **Koden gör mer än den behöver.**
  - Varför det händer: det är lätt att börja bygga hälsa, animation och avancerad AI samtidigt.
  - Hur man undviker det: låt `EnemyPatrol` sköta rörelse och `DamageZone` sköta farlig kontakt.

## Övningar

### Övning 1: Justera patrullen

Ändra `moveSpeed` på fienden och testa hur banan känns.

Prova minst tre värden:

- långsam fiende
- medelsnabb fiende
- för snabb fiende

Skriv ner vilket värde som känns bäst och varför.

### Övning 2: Skapa två olika hinder

Skapa två farliga objekt som båda använder `DamageZone`, till exempel:

- taggar på marken
- en farlig buske
- en osynlig fallzon under banan

Båda ska skriva meddelandet i Console när spelaren rör vid dem.

### Övning 3: Gör patrullen längre

Flytta `PatrolPointA` och `PatrolPointB` så att fienden täcker en större del av plattformen.

Testa sedan:

- blir banan mer intressant?
- blir den orättvis?
- finns det en tydlig säker plats där spelaren kan vänta?

### Fördjupning

Lägg till en enkel vändning av fiendens sprite när den byter riktning.

Tips: du kan ändra `transform.localScale.x`, men gör det försiktigt så att du inte råkar ändra storlek på hela objektet på ett oväntat sätt.

## Snabb sammanfattning

- En fiende i Unity är ett GameObject med components och scripts.
- Patrullering kan göras genom att flytta ett objekt mellan två `Transform`-punkter.
- `Vector2.MoveTowards` passar bra för enkel rörelse mot ett mål.
- En trigger upptäcker kontakt utan att fungera som en fysisk vägg.
- `OnTriggerEnter2D` kan användas för att reagera när spelaren rör vid en fiende eller ett hinder.
- Hälsa och respawn väntar vi med till nästa kapitel.

## Quiz/reflektionsfrågor

1. Varför använder vi två tomma GameObjects som patrullpunkter?
2. Vad är skillnaden mellan en vanlig collider och en trigger?
3. Varför är det bra att `EnemyPatrol` och `DamageZone` är två separata scripts?
4. Vad händer om spelaren inte har taggen `Player`?
5. Hur kan en fiende göra en bana mer intressant utan att göra den orättvis?

## Nästa steg

Nu kan spelet upptäcka när spelaren rör vid fiender och hinder. Nästa steg är att ge den kontakten en tydlig konsekvens. I nästa kapitel bygger vi hälsa, liv och checkpoints så att spelaren kan misslyckas, återvända och försöka igen.
