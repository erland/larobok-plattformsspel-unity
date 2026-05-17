# Kapitel 10: Samla objekt och visa poäng

## Varför detta kapitel finns

I de senaste kapitlen har Skogssprånget fått rörelse, bana, kamera, fiender, hinder, hälsa och checkpoints. Spelet börjar kännas som ett riktigt plattformsspel, men det saknar fortfarande en viktig sak: belöningar.

I många plattformsspel finns objekt att samla. De kan vara mynt, frukter, stjärnor, nycklar eller något som passar spelets tema. De ger spelaren en anledning att utforska banan och hjälper oss som utvecklare att öva på ett viktigt programmeringsmönster: när något händer i spelet ska ett värde ändras och spelaren ska få återkoppling.

I det här kapitlet skapar vi samlingsobjekt, räknar poäng och visar poängen med enkel UI-text.

## Lärandemål

Efter kapitlet ska du kunna:

- skapa ett samlingsobjekt som reagerar när spelaren rör vid det
- hålla reda på spelarens poäng i ett separat script
- visa poäng på skärmen med ett enkelt UI
- låta olika scripts samarbeta utan att blanda ihop deras ansvar
- testa att spelaren får tydlig återkoppling när något samlas in

## Innan vi börjar

Du behöver projektet från föregående kapitel. Det ska innehålla:

- en spelare med collider och taggen `Player`
- en bana med plattformar
- en kamera som visar spelaren
- ett fungerande hälsosystem
- en scen där du kan placera ut nya objekt

Vi kommer att skapa tre nya delar:

1. ett objekt som kan samlas in
2. ett script som håller reda på poängen
3. en UI-text som visar poängen

## Huvudförklaring

### Vad är ett samlingsobjekt?

Ett samlingsobjekt är ett objekt i spelet som försvinner eller förändras när spelaren plockar upp det.

I Skogssprånget kan vi tänka oss små skogsbär, löv eller ljusglimtar. För enkelhetens skull kallar vi dem `Collectible`.

Ett samlingsobjekt behöver vanligtvis:

- en synlig sprite
- en collider som kan upptäcka kontakt
- ett script som reagerar när spelaren rör vid objektet
- ett poängvärde

Det ska inte själv behöva veta allt om spelets regler. Dess ansvar är bara att säga: "Spelaren plockade upp mig, ge poäng och ta bort mig."

### Skapa ett enkelt samlingsobjekt

Skapa ett nytt tomt GameObject i scenen och döp det till `Berry`.

Lägg till:

- `Sprite Renderer`
- en valfri sprite
- `CircleCollider2D` eller `BoxCollider2D`

Markera collidern som **Is Trigger**. Det betyder att objektet upptäcker kontakt utan att stoppa spelaren fysiskt.

Placera objektet ovanför en plattform så att spelaren kan hoppa in i det.

### Scriptet Collectible

Skapa ett nytt C#-script som heter `Collectible`.

Koppla scriptet till objektet `Berry`.

```csharp
using UnityEngine;

public class Collectible : MonoBehaviour
{
    [SerializeField] private int points = 1;

    private void OnTriggerEnter2D(Collider2D other)
    {
        if (!other.CompareTag("Player"))
        {
            return;
        }

        ScoreManager.Instance.AddPoints(points);
        Destroy(gameObject);
    }
}
```

Det här scriptet gör tre saker:

1. kontrollerar om objektet som rörde vid samlingsobjektet är spelaren
2. lägger till poäng
3. tar bort samlingsobjektet från scenen

Raden `Destroy(gameObject)` betyder att Unity tar bort just det GameObject som scriptet sitter på.

### Varför använder vi ScoreManager?

Vi skulle kunna lägga poängen direkt i `Collectible`, men då skulle varje samlingsobjekt ha sin egen poängräknare. Det är inte vad vi vill.

Poängen hör till spelet som helhet, inte till ett enskilt bär.

Därför skapar vi ett separat script: `ScoreManager`.

Det scriptet ska ha ansvar för:

- aktuell poäng
- att lägga till nya poäng
- att uppdatera UI-texten

### Skapa ScoreManager

Skapa ett tomt GameObject i scenen och döp det till `ScoreManager`.

Skapa sedan ett C#-script med samma namn och koppla det till objektet.

```csharp
using TMPro;
using UnityEngine;

public class ScoreManager : MonoBehaviour
{
    public static ScoreManager Instance { get; private set; }

    [SerializeField] private TMP_Text scoreText;

    private int score;

    private void Awake()
    {
        if (Instance != null && Instance != this)
        {
            Destroy(gameObject);
            return;
        }

        Instance = this;
    }

    private void Start()
    {
        UpdateScoreText();
    }

    public void AddPoints(int points)
    {
        score += points;
        UpdateScoreText();
    }

    private void UpdateScoreText()
    {
        scoreText.text = "Poäng: " + score;
    }
}
```

Det här är det mest avancerade scriptet hittills i boken, så vi tar det steg för steg.

### Instance i enkel form

Raden nedan gör att andra scripts kan hitta samma `ScoreManager`:

```csharp
public static ScoreManager Instance { get; private set; }
```

Detta är en enkel variant av ett mönster som ofta kallas **singleton**. I den här boken använder vi det försiktigt, bara för att göra ett litet projekt enklare att följa.

Tanken är:

- det ska finnas en aktiv `ScoreManager` i scenen
- andra scripts kan använda `ScoreManager.Instance`
- samlingsobjekt behöver inte själva hålla reda på poängen

I större projekt finns fler sätt att lösa detta. Just nu väljer vi den här lösningen eftersom den är tydlig och praktisk.

### Skapa UI-texten

Nu behöver vi något som visar poängen på skärmen.

Gör så här:

1. Högerklicka i Hierarchy.
2. Välj **UI** och skapa ett textobjekt med TextMeshPro om Unity frågar efter det.
3. Döp textobjektet till `ScoreText`.
4. Placera texten uppe till vänster i Game-vyn.
5. Skriv en starttext, till exempel `Poäng: 0`.

När du skapar ett UI-objekt skapar Unity vanligtvis också ett `Canvas`. Canvas är den yta där UI visas.

Markera sedan `ScoreManager` i Hierarchy och dra `ScoreText` till fältet `Score Text` i Inspector.

### Testa samlingen

Tryck på Play och styr spelaren in i samlingsobjektet.

Kontrollera att:

- objektet försvinner
- poängen ökar
- texten på skärmen uppdateras

Om något inte fungerar, börja med Console. Felmeddelanden är ofta den snabbaste vägen till problemet.

## Exempel: flera bär med olika poäng

Du kan duplicera `Berry` flera gånger och placera ut kopior på banan.

Eftersom varje `Collectible` har ett fält som heter `Points` kan du ge olika objekt olika värde.

Exempel:

| Objekt | Points | Användning |
|---|---:|---|
| Litet bär | 1 | Vanlig belöning |
| Gyllene bär | 5 | Svårare placering |
| Hemlig glimt | 10 | Bonus för utforskning |

Det här gör att du kan styra hur spelaren rör sig genom banan. Ett samlingsobjekt högt uppmuntrar hopp. Ett objekt nära en fiende skapar risk och belöning. Ett objekt bakom en svår väg uppmuntrar utforskning.

## Vanliga misstag

### Score Text är inte kopplad

Om Console visar ett fel som säger att något är `null` kan det bero på att `Score Text` inte är kopplat i Inspector.

Lösning:

1. Markera `ScoreManager`.
2. Leta upp fältet `Score Text`.
3. Dra in textobjektet från Hierarchy.

### Collidern är inte en trigger

Om spelaren krockar med samlingsobjektet i stället för att plocka upp det är `Is Trigger` troligen inte markerat.

Lösning:

- markera samlingsobjektet
- hitta dess collider
- markera `Is Trigger`

### Spelaren saknar taggen Player

`Collectible` använder `CompareTag("Player")`. Om spelarobjektet inte har taggen `Player` händer ingenting.

Lösning:

- markera spelaren
- kontrollera fältet `Tag` högst upp i Inspector
- välj `Player`

### Det finns ingen ScoreManager i scenen

Om ett samlingsobjekt försöker anropa `ScoreManager.Instance` men ingen `ScoreManager` finns i scenen uppstår fel.

Lösning:

- skapa ett tomt GameObject
- döp det till `ScoreManager`
- koppla scriptet `ScoreManager`
- koppla `ScoreText`

## Övningar

### Övning 1: Placera ut fem samlingsobjekt

Placera ut minst fem bär i banan. Gör några lätta att ta och några som kräver hopp eller risk.

Testa sedan:

- att alla går att samla
- att poängen ökar varje gång
- att objekten försvinner efter kontakt

### Övning 2: Skapa ett bonusobjekt

Skapa en kopia av ditt samlingsobjekt och ge den ett högre poängvärde.

Förslag:

- vanligt bär: 1 poäng
- bonusbär: 5 poäng

Placera bonusobjektet på en svårare plats.

### Fördjupning: Lägg till enkel ljudåterkoppling

Om du vill experimentera kan du lägga till ett ljud när spelaren samlar ett objekt.

Det kräver att du senare arbetar mer med ljud, så håll det enkelt:

- skapa först en fungerande poängräkning
- lägg till ljud först när grundfunktionen fungerar
- dokumentera vad du ändrar

## Snabb sammanfattning

- Ett samlingsobjekt är ett objekt som reagerar när spelaren rör vid det.
- `Collectible` ansvarar för att ge poäng och ta bort objektet.
- `ScoreManager` ansvarar för spelarens poäng och UI-text.
- `Canvas` används för UI i scenen.
- `TMP_Text` används för att ändra TextMeshPro-text från kod.
- Ett tydligt ansvar per script gör projektet lättare att felsöka.

## Quiz/reflektionsfrågor

1. Varför ska varje samlingsobjekt inte ha sin egen totala poängräknare?
2. Vad gör `Destroy(gameObject)`?
3. Varför behöver `ScoreText` kopplas i Inspector?
4. Vad är skillnaden mellan ett vanligt kollisionsobjekt och en trigger?
5. När kan det vara bra att ge olika samlingsobjekt olika poängvärde?

## Nästa steg

Nu kan spelaren samla objekt och få poäng. Spelet har rörelse, risk, belöning och återkoppling.

I nästa kapitel arbetar vi med animationer och spelkänsla. Då ska spelarens rörelser kännas tydligare och spelet börja upplevas mer levande.
