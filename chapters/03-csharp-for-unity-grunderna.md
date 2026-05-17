# Kapitel 3: C# för Unity: grunderna du behöver

## Varför detta kapitel finns

Hittills har du skapat ett Unity-projekt och lärt känna arbetsytan. Du har sett att ett spel i Unity byggs av scener, GameObjects och Components. Nästa steg är att ge objekten egna beteenden.

Det är där C# kommer in. I Unity skriver vi C#-kod i scripts. Ett script kan till exempel säga: “när spelet startar, skriv ett meddelande”, “när spelaren trycker på en tangent, flytta objektet” eller “när spelaren samlar ett mynt, öka poängen”.

I det här kapitlet ska du inte lära dig hela C#. Målet är att ge dig precis den grund du behöver för att förstå Unity-scripts innan vi börjar bygga spelarens rörelse i nästa kapitel.

## Lärandemål

Efter kapitlet ska du kunna:

- förklara vad ett script är i Unity
- skapa ett enkelt C#-script och koppla det till ett GameObject
- känna igen variabler, metoder och klasser
- använda `Start()` och `Update()` på en grundläggande nivå
- ändra publika värden i Inspector
- läsa enkla felmeddelanden utan att få panik

## Innan vi börjar

I kapitel 2 introducerades tre viktiga begrepp:

- **Scene**: en avgränsad del av spelet, till exempel en nivå eller testmiljö.
- **GameObject**: ett objekt i scenen.
- **Component**: en del som ger ett GameObject egenskaper eller beteende.

Ett C#-script blir en egen Component när du kopplar det till ett GameObject. Det betyder att kod inte lever fritt i Unity. Den sitter nästan alltid på ett objekt och körs som en del av objektets beteende.

## Huvudförklaring

### Script är beteende

Tänk på ett GameObject som en tom figur på en scen. Figuren har en Transform, så Unity vet var den finns. Den kan ha en Sprite Renderer, så den syns. Den kan ha en Collider, så den kan krocka med andra saker.

Men om figuren ska göra något behöver den beteende. Ett script är ett sätt att skapa sådant beteende.

Exempel på beteenden:

- en spelare som rör sig
- ett mynt som försvinner när det samlas in
- en fiende som patrullerar
- en dörr som öppnas
- en menyknapp som startar spelet

I Unity är ett script en C#-fil. Filen har oftast samma namn som klassen inuti filen. Det är viktigt, eftersom Unity förväntar sig den kopplingen.

### Skapa ett första script

Skapa gärna en ny testscen eller fortsätt i den testscen du byggde i kapitel 2.

Gör så här:

1. Skapa ett tomt GameObject i scenen.
2. Döp objektet till `TestObject`.
3. Skapa en mapp i Project-vyn som heter `Scripts`.
4. Högerklicka i mappen `Scripts`.
5. Välj att skapa ett nytt C# script.
6. Döp scriptet till `FirstScript`.
7. Dra scriptet till `TestObject` i Hierarchy eller använd Add Component i Inspector.

Öppna sedan scriptet i din kodredigerare. Ett nytt Unity-script brukar se ungefär ut så här:

```csharp
using UnityEngine;

public class FirstScript : MonoBehaviour
{
    void Start()
    {

    }

    void Update()
    {

    }
}
```

Det här kan se formellt ut, men du behöver inte förstå allt på en gång. Vi tar det bit för bit.

### Klassen är scriptets behållare

Raden här skapar en klass:

```csharp
public class FirstScript : MonoBehaviour
```

En **klass** är en behållare för kod som hör ihop. I Unity motsvarar klassen ofta en typ av beteende.

I det här fallet betyder `FirstScript` ungefär: “det här är beteendet som heter FirstScript”.

Delen `: MonoBehaviour` betyder att klassen är ett Unity-beteende som kan användas som en Component på ett GameObject. Utan `MonoBehaviour` skulle scriptet inte fungera på samma sätt i Unitys vanliga GameObject-arbetsflöde.

En viktig regel:

- Filen heter `FirstScript.cs`.
- Klassen heter `FirstScript`.

Om de inte matchar kan Unity inte alltid använda scriptet som en Component.

### Metoder är namngivna instruktioner

Inuti klassen finns två metoder:

```csharp
void Start()
{

}

void Update()
{

}
```

En **metod** är ett namngivet block med instruktioner. När metoden körs körs instruktionerna mellan klamrarna.

`Start()` körs en gång när scriptet startar.

`Update()` körs om och om igen, ungefär en gång per bildruta medan spelet körs.

Det gör att de passar för olika saker:

| Metod | När körs den? | Passar för |
|---|---|---|
| `Start()` | En gång i början | Startvärden, testmeddelanden, enkel förberedelse |
| `Update()` | Varje bildruta | Input, kontroll av tangenter, saker som ändras hela tiden |

Om något bara behöver göras en gång, börja med `Start()`. Om något behöver kontrolleras hela tiden, använd `Update()`.

### Skriv ett meddelande till Console

Unity har ett Console-fönster där du kan se meddelanden, varningar och fel.

Lägg in detta i `Start()`:

```csharp
using UnityEngine;

public class FirstScript : MonoBehaviour
{
    void Start()
    {
        Debug.Log("Skogssprånget har startat!");
    }

    void Update()
    {

    }
}
```

Spara filen, gå tillbaka till Unity och tryck på Play. I Console bör du se meddelandet.

Det här är ett enkelt men viktigt verktyg. `Debug.Log()` hjälper dig att kontrollera vad som händer i spelet. Det är som att lämna små lappar till dig själv medan spelet körs.

### Variabler sparar värden

En **variabel** är ett namn på ett värde som programmet kan använda.

Exempel:

```csharp
int coins = 0;
float moveSpeed = 5.0f;
string playerName = "Player";
bool isGrounded = true;
```

Varje rad har tre delar:

- typ
- namn
- värde

| Exempel | Typ | Namn | Värde |
|---|---|---|---|
| `int coins = 0;` | heltal | `coins` | `0` |
| `float moveSpeed = 5.0f;` | decimaltal | `moveSpeed` | `5.0f` |
| `string playerName = "Player";` | text | `playerName` | `"Player"` |
| `bool isGrounded = true;` | sant/falskt | `isGrounded` | `true` |

I Unity används `float` ofta för hastighet, tid, avstånd och andra värden som kan ha decimaler.

Bokstaven `f` i `5.0f` betyder att talet ska behandlas som en `float`.

### Publika variabler syns i Inspector

Om du skriver `public` framför en variabel kan Unity visa den i Inspector.

Ändra scriptet till detta:

```csharp
using UnityEngine;

public class FirstScript : MonoBehaviour
{
    public float moveSpeed = 5.0f;

    void Start()
    {
        Debug.Log("Move speed is: " + moveSpeed);
    }

    void Update()
    {

    }
}
```

Spara och gå tillbaka till Unity. Markera `TestObject`. Nu bör du se fältet **Move Speed** i Inspector.

Det här är en av Unitys viktigaste arbetsmetoder:

- Koden definierar vilka värden som finns.
- Inspector låter dig justera värden utan att ändra koden varje gång.

I senare kapitel kommer vi använda det här för spelarens hastighet, hoppstyrka, kamerainställningar och andra spelvärden.

### Kod körs uppifrån och ned

När en metod körs läser datorn instruktionerna i ordning. Det är lätt att glömma när man är ny.

Exempel:

```csharp
void Start()
{
    int coins = 0;
    coins = coins + 1;
    Debug.Log(coins);
}
```

Först skapas `coins` med värdet `0`. Sedan ökas värdet med `1`. Sedan skrivs värdet ut.

Console visar därför:

```text
1
```

När du felsöker kod är det ofta bra att fråga:

- Vilken rad körs först?
- Vilket värde har variabeln just nu?
- Ändras värdet någonstans innan det används?

### Ett script kan ha egna metoder

Du kan skapa egna metoder för att dela upp kod i mindre delar.

Exempel:

```csharp
using UnityEngine;

public class FirstScript : MonoBehaviour
{
    public int coins = 0;

    void Start()
    {
        AddCoin();
        Debug.Log("Coins: " + coins);
    }

    void AddCoin()
    {
        coins = coins + 1;
    }
}
```

Metoden `AddCoin()` ökar värdet på `coins`. I `Start()` anropar vi metoden genom att skriva dess namn följt av parenteser.

Det här är grunden till att strukturera kod. I stället för att lägga allt på ett ställe kan du dela upp beteenden i små metoder med tydliga namn.

### Namn ska hjälpa dig tänka

Bra namn gör kod lättare att förstå.

Mindre bra:

```csharp
public float x = 5.0f;
```

Bättre:

```csharp
public float moveSpeed = 5.0f;
```

Mindre bra:

```csharp
void DoThing()
{

}
```

Bättre:

```csharp
void AddCoin()
{

}
```

När du väljer namn, fråga dig: “Skulle jag förstå det här om jag öppnade projektet om två veckor?”

## Exempel

Nu bygger vi ett litet script som hör ihop med vårt exempelspel Skogssprånget. Det ska inte styra spelaren ännu. Det ska bara hålla reda på några enkla spelarvärden och skriva ut dem.

Skapa ett nytt script som heter `PlayerInfo`.

Koppla det till ett GameObject som heter `PlayerTest`.

Skriv koden så här:

```csharp
using UnityEngine;

public class PlayerInfo : MonoBehaviour
{
    public string playerName = "Forest Runner";
    public int coins = 0;
    public float moveSpeed = 5.0f;
    public bool isGrounded = true;

    void Start()
    {
        PrintPlayerInfo();
    }

    void PrintPlayerInfo()
    {
        Debug.Log("Player: " + playerName);
        Debug.Log("Coins: " + coins);
        Debug.Log("Move speed: " + moveSpeed);
        Debug.Log("Is grounded: " + isGrounded);
    }
}
```

Tryck på Play och titta i Console.

Testa sedan att ändra värdena i Inspector:

- ändra `Player Name`
- ändra `Coins`
- ändra `Move Speed`
- kryssa i eller ur `Is Grounded`

Tryck på Play igen. Console bör visa de nya värdena.

Det här visar en viktig koppling mellan kod och editor. Scriptet definierar beteendet, men Inspector gör det möjligt att experimentera snabbt.

## Vanliga misstag

- **Misstag: Filnamn och klassnamn matchar inte.**
  - Varför det händer: Scriptet döptes om i Unity eller i kodredigeraren utan att klassnamnet ändrades.
  - Hur man undviker det: Kontrollera att `PlayerInfo.cs` innehåller `public class PlayerInfo`.

- **Misstag: Semikolon saknas.**
  - Varför det händer: Många C#-rader måste sluta med `;`.
  - Hur man undviker det: Titta på raden som Unity markerar, men kontrollera även raden ovanför.

- **Misstag: Koden sparades inte innan Unity testades.**
  - Varför det händer: Kodredigeraren och Unity är två separata program.
  - Hur man undviker det: Spara alltid filen innan du går tillbaka till Unity.

- **Misstag: Scriptet finns i Project-vyn men sitter inte på något GameObject.**
  - Varför det händer: Scriptet skapades men kopplades aldrig som Component.
  - Hur man undviker det: Markera objektet och kontrollera Inspector.

- **Misstag: För mycket kod skrivs på en gång.**
  - Varför det händer: Man vill snabbt bygga något färdigt.
  - Hur man undviker det: Skriv några få rader, spara, testa och fortsätt sedan.

## Övningar

### Övning 1: Skapa ett eget testscript

Skapa ett script som heter `GameSettings`.

Lägg till dessa publika variabler:

```csharp
public int startLives = 3;
public float gravityScale = 2.5f;
public string levelName = "Forest Start";
```

Skriv ut värdena i `Start()` med `Debug.Log()`.

Testa att ändra värdena i Inspector och kör scenen igen.

### Övning 2: Lägg till en egen metod

Utöka `GameSettings` med en metod som heter `PrintSettings()`.

Flytta dina `Debug.Log()`-rader från `Start()` till `PrintSettings()` och anropa metoden från `Start()`.

Målet är att öva på att dela upp kod i namngivna delar.

### Övning 3: Läs ett felmeddelande

Skapa medvetet ett litet fel genom att ta bort ett semikolon från en rad.

Gå tillbaka till Unity och öppna Console.

Svara på frågorna:

1. Vilken fil nämns i felmeddelandet?
2. Vilken rad nämns?
3. Vad tror du Unity försöker säga?
4. Vad händer när du lägger tillbaka semikolonet och sparar?

### Fördjupning

Skapa ett script som heter `CoinCounter`.

Det ska ha:

- en publik `int` som heter `coins`
- en metod som heter `AddCoin()`
- ett `Debug.Log()`-meddelande som visar antal coins efter att ett coin lagts till

Anropa `AddCoin()` tre gånger från `Start()` och kontrollera att värdet ökar.

## Snabb sammanfattning

- Ett Unity-script är en C#-fil som kan fungera som en Component.
- En klass är en behållare för kod som hör ihop.
- En metod är ett namngivet block med instruktioner.
- `Start()` körs en gång när scriptet startar.
- `Update()` körs varje bildruta.
- En variabel sparar ett värde.
- Publika variabler kan visas och ändras i Inspector.
- `Debug.Log()` hjälper dig att förstå vad som händer när spelet körs.

## Quiz och reflektionsfrågor

1. Vad är skillnaden mellan ett GameObject och ett script?
2. Varför måste filnamn och klassnamn ofta matcha i Unity?
3. När passar det bäst att använda `Start()`?
4. När passar det bäst att använda `Update()`?
5. Vad är fördelen med att göra en variabel `public` i ett Unity-script?
6. Varför är bra namn på variabler och metoder viktiga?
7. Hur kan `Debug.Log()` hjälpa dig när något inte fungerar?

## Nästa steg

Nu har du den C#-grund som behövs för att börja skapa riktig spelmekanik. I nästa kapitel använder vi variabler, metoder och Unitys uppdateringsflöde för att ge spelaren horisontell rörelse.

Då går vi från att läsa och skriva värden till att faktiskt styra ett objekt i spelet.
