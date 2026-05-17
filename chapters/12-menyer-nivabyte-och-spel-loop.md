# Kapitel 12: Menyer, nivåbyte och spel-loop

## Varför detta kapitel finns

Hittills har Skogssprånget framför allt varit en spelbar testbana. Spelaren kan röra sig, hoppa, ta skada, samla objekt och få återkoppling. Det är en viktig milstolpe, men ett spel behöver också en tydlig ram runt själva spelandet.

En spelare ska kunna starta spelet från en meny, förstå när en nivå är klar och kunna försöka igen om något går fel. Det är detta som brukar kallas en spel-loop: spelaren startar, spelar, lyckas eller misslyckas och får sedan ett tydligt nästa steg.

I det här kapitlet bygger vi en enkel startmeny, en spelbar nivå och en tydlig väg tillbaka efter vinst eller misslyckande.

## Lärandemål

Efter kapitlet ska du kunna:

- förklara vad en spel-loop är i ett enkelt plattformsspel
- skapa flera scener i ett Unity-projekt
- byta scen med `SceneManager`
- skapa en enkel startmeny med UI-knappar
- skapa enkel nivåavslutning och omstart
- undvika vanliga problem med scenordning och saknade referenser

## Innan vi börjar

Du behöver projektet från föregående kapitel. Det ska innehålla:

- en spelbar nivå
- en spelare som kan röra sig, hoppa och ta skada
- samlingsobjekt och poäng
- enkel UI-feedback
- gärna animationer eller annan spelkänsla från kapitel 11

I det här kapitlet använder vi Unitys vanliga scener och UI-system. Vi håller lösningen enkel så att du får ett stabilt flöde innan projektet växer.

## Huvudförklaring

### Vad är en spel-loop?

En spel-loop är inte samma sak som `Update()`-loopen i Unity. Här menar vi spelets övergripande flöde.

I Skogssprånget kan en enkel spel-loop se ut så här:

1. Spelaren kommer till en startmeny.
2. Spelaren trycker på Start.
3. Nivån laddas.
4. Spelaren försöker nå målet.
5. Om spelaren lyckas visas nästa steg.
6. Om spelaren misslyckas kan nivån startas om.
7. Spelaren kan spela igen eller återgå till menyn.

Detta gör spelet mer komplett. Även en liten prototyp känns mer färdig när spelaren vet hur den börjar, slutar och startar om.

### Scener som delar av spelet

En **Scene** kan vara en nivå, en meny eller en testmiljö. Tidigare har vi främst använt en scen som spelbana. Nu delar vi upp projektet tydligare.

Skapa gärna följande scener:

```text
MainMenu
Level01
GameOver
```

Du kan också börja med bara två scener:

```text
MainMenu
Level01
```

I den enklare varianten kan `Level01` själv hantera omstart och vinstmeddelanden. I den här boken använder vi den enklare varianten först, eftersom den är lättare att förstå.

### Skapa en startmeny

Skapa en ny scen och spara den som:

```text
MainMenu
```

Skapa sedan ett Canvas:

```text
GameObject > UI > Canvas
```

Lägg till en rubrik och en knapp:

```text
GameObject > UI > Text - TextMeshPro
GameObject > UI > Button - TextMeshPro
```

Döp knappen till:

```text
StartButton
```

Skriv till exempel:

```text
Starta spel
```

Om Unity frågar om du vill importera TextMeshPro Essentials, välj att göra det. TextMeshPro är Unitys vanliga system för tydlig text i UI.

### Lägg scener i Build Settings

För att Unity ska kunna byta scen måste scenerna finnas i Build Settings.

Öppna:

```text
File > Build Settings
```

Lägg till scenerna i listan **Scenes In Build**:

```text
MainMenu
Level01
```

Ordningen spelar roll om du laddar scener med index. I den här boken laddar vi scener med namn, men det är ändå bra att ha en tydlig ordning:

| Index | Scen |
|---|---|
| 0 | MainMenu |
| 1 | Level01 |

Spara båda scenerna innan du testar.

### Skapa ett MenuController-script

Skapa ett script som heter:

```text
MenuController
```

Lägg det på ett tomt GameObject i `MainMenu`. Döp objektet till:

```text
MenuController
```

Skriv följande kod:

```csharp
using UnityEngine;
using UnityEngine.SceneManagement;

public class MenuController : MonoBehaviour
{
    public void StartGame()
    {
        SceneManager.LoadScene("Level01");
    }

    public void QuitGame()
    {
        Application.Quit();
    }
}
```

Metoden `StartGame()` laddar scenen `Level01`. Metoden `QuitGame()` avslutar spelet när det är byggt som riktig applikation. I Unity Editor händer det ofta inget synligt när `Application.Quit()` körs, och det är normalt.

### Koppla knappen till koden

Markera `StartButton` i Hierarchy.

I knappens `Button`-komponent hittar du listan **On Click**.

Gör så här:

1. Tryck på plusknappen.
2. Dra in objektet `MenuController` i fältet.
3. Välj `MenuController > StartGame`.

Starta scenen `MainMenu` och testa knappen. Om allt är rätt ska `Level01` laddas.

### Skapa ett mål i nivån

Nu behöver nivån veta när spelaren har klarat den.

I `Level01`, skapa ett GameObject nära slutet av banan. Döp det till:

```text
Goal
```

Ge det en `BoxCollider2D` och markera **Is Trigger**.

Skapa sedan scriptet:

```text
GoalTrigger
```

Använd den här koden:

```csharp
using UnityEngine;

public class GoalTrigger : MonoBehaviour
{
    [SerializeField] private GameObject levelCompletePanel;

    private void OnTriggerEnter2D(Collider2D other)
    {
        if (!other.CompareTag("Player"))
        {
            return;
        }

        levelCompletePanel.SetActive(true);
        Time.timeScale = 0f;
    }
}
```

Det här scriptet visar en panel när spelaren når målet och pausar spelet med `Time.timeScale = 0f`.

### Skapa en Level Complete-panel

I `Level01`, skapa ett UI-panelobjekt i Canvas och döp det till:

```text
LevelCompletePanel
```

Panelen kan innehålla:

- texten `Banan klar!`
- en knapp för att spela igen
- en knapp för att gå till menyn

Stäng av panelen i Inspector genom att avmarkera den högst upp i objektets ruta. Den ska alltså vara dold när nivån börjar.

Dra sedan panelen till fältet `Level Complete Panel` i `GoalTrigger`.

### Skapa en LevelController

Nu behöver knapparna på panelen kunna starta om nivån eller gå tillbaka till menyn.

Skapa ett script:

```text
LevelController
```

Lägg det på ett tomt GameObject i `Level01` och döp objektet till:

```text
LevelController
```

Skriv följande kod:

```csharp
using UnityEngine;
using UnityEngine.SceneManagement;

public class LevelController : MonoBehaviour
{
    public void RestartLevel()
    {
        Time.timeScale = 1f;
        SceneManager.LoadScene("Level01");
    }

    public void ReturnToMainMenu()
    {
        Time.timeScale = 1f;
        SceneManager.LoadScene("MainMenu");
    }
}
```

Koppla sedan UI-knapparna:

- knappen `Spela igen` ska anropa `LevelController.RestartLevel`
- knappen `Till meny` ska anropa `LevelController.ReturnToMainMenu`

Lägg märke till att vi sätter `Time.timeScale = 1f` innan vi laddar en scen. Det är viktigt. Om spelet är pausat och du byter scen utan att återställa tiden kan nästa scen upplevas som frusen.

### Omstart vid Game Over

Om du redan har ett system där spelaren tar skada och återvänder till en checkpoint kan du låta det fortsätta. En fullständig game over behövs inte alltid i ett plattformsspel med checkpoints.

Men om du vill ha en enkel game over kan du använda samma princip som för `LevelCompletePanel`:

- skapa en `GameOverPanel`
- visa panelen när spelarens hälsa når noll
- pausa spelet
- låt knappar starta om nivån eller gå till menyn

Det viktiga är att inte blanda för många lösningar samtidigt. I början räcker det med en tydlig omstartsknapp.

### Paus, menyer och tid

`Time.timeScale` påverkar många tidsbaserade saker i Unity. När värdet är `0f` står spelets fysik och många uppdateringar stilla. Det passar bra för en enkel paus eller en vinstpanel.

Men UI-knappar fungerar fortfarande, vilket gör att spelaren kan klicka på `Spela igen` eller `Till meny`.

Kom ihåg regeln:

- pausa spelet med `Time.timeScale = 0f`
- återställ alltid med `Time.timeScale = 1f` innan du fortsätter eller laddar om

### Namn är viktiga

När du använder:

```csharp
SceneManager.LoadScene("Level01");
```

måste scenens namn stavas exakt likadant som scenfilen i projektet. Skillnad mellan `Level01`, `Level1` och `level01` kan skapa fel.

En bra vana är att hålla scennamn korta, tydliga och konsekventa:

```text
MainMenu
Level01
Level02
GameOver
```

## Exempel: Skogssprångets första spel-loop

I Skogssprånget kan flödet nu vara:

1. `MainMenu` visas.
2. Spelaren trycker på `Starta spel`.
3. `Level01` laddas.
4. Spelaren samlar mynt och tar sig förbi hinder.
5. Spelaren når `Goal`.
6. `LevelCompletePanel` visas.
7. Spelaren väljer `Spela igen` eller `Till meny`.

Detta är en liten men komplett struktur. Spelet har nu en början, en spelbar mitt och ett tydligt avslut.

## Vanliga misstag

- **Misstag: Scenen laddas inte.**
  - Varför det händer: Scenen saknas i Build Settings eller namnet är felstavat.
  - Hur man undviker det: Lägg till alla scener i **Scenes In Build** och använd exakt samma namn i koden.

- **Misstag: Spelet är fruset efter omstart.**
  - Varför det händer: `Time.timeScale` sattes till `0f` men återställdes inte.
  - Hur man undviker det: Sätt alltid `Time.timeScale = 1f` innan du laddar om nivån eller återgår till menyn.

- **Misstag: Knappen gör inget.**
  - Varför det händer: Knappen är inte kopplad till rätt objekt och metod i **On Click**.
  - Hur man undviker det: Kontrollera att rätt GameObject ligger i fältet och att rätt public method är vald.

- **Misstag: Vinstpanelen syns från början.**
  - Varför det händer: Panelen är aktiv i scenen när spelet startar.
  - Hur man undviker det: Avmarkera panelens GameObject i Inspector och aktivera den först via script.

## Övningar

### Övning 1: Skapa startmenyn

Skapa scenen `MainMenu` med:

- en titel
- en startknapp
- ett `MenuController`-script

Testa att startknappen laddar `Level01`.

### Övning 2: Skapa målet

Lägg till ett `Goal`-objekt i slutet av banan.

Kontrollera att:

- objektet har en trigger-collider
- spelaren har taggen `Player`
- `LevelCompletePanel` visas när spelaren når målet

### Övning 3: Lägg till omstart

Skapa en knapp som laddar om `Level01`.

Testa särskilt att spelet inte är pausat efter omstart.

### Fördjupning

Skapa en extra scen:

```text
Level02
```

Låt `GoalTrigger` ladda `Level02` i stället för att visa en vinstpanel. Fundera på vad som behöver ändras om spelet ska ha flera nivåer.

## Snabb sammanfattning

- En spel-loop beskriver spelets övergripande flöde från start till omstart eller avslut.
- Scener kan användas för menyer, nivåer och andra delar av spelet.
- `SceneManager.LoadScene()` laddar en scen med namn.
- Scener måste finnas i Build Settings för att kunna laddas i ett byggt spel.
- UI-knappar kopplas till publika metoder via **On Click**.
- `Time.timeScale = 0f` kan pausa spelet, men bör alltid återställas till `1f`.

## Quiz/reflektionsfrågor

1. Vad är skillnaden mellan spelets övergripande spel-loop och Unitys `Update()`?
2. Varför behöver scener läggas till i Build Settings?
3. Vad händer om du glömmer att återställa `Time.timeScale`?
4. Varför är det bättre att börja med ett enkelt menyflöde än ett avancerat menysystem?
5. Vilka scener skulle ditt spel behöva om det fick tre nivåer?

## Nästa steg

Spelet börjar nu kännas som en riktig prototyp. Det har en meny, en spelbar nivå, ett mål och möjlighet att spela igen. Nästa kapitel handlar om något som blir allt viktigare när projektet växer: versionshantering.

Då lär du dig hur du sparar projektets utveckling i Git, hur Unity-projekt bör förberedas för GitHub och varför commits gör det tryggare att experimentera.
