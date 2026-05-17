# Kapitel 13: Versionshantera Unity-projekt med Git och GitHub

## Varför detta kapitel finns

När ett Unity-projekt växer blir det snabbt svårt att hålla reda på ändringar. Du testar en ny fiende, ändrar spelarens rörelse, råkar ta bort en fil eller vill jämföra dagens version med gårdagens. Utan versionshantering finns ofta bara ett alternativ: kopiera hela projektmappen och hoppas att du kommer ihåg vilken kopia som fungerar.

Git löser det problemet. Med Git kan du spara tydliga ändringspunkter, gå tillbaka till tidigare versioner och arbeta mer strukturerat. GitHub, eller en liknande tjänst, låter dig dessutom lagra projektet online och dela det med andra.

I det här kapitlet versionshanterar vi Skogssprånget på ett sätt som passar Unity-projekt. Målet är inte att du ska bli Git-expert, utan att du ska få ett tryggt arbetsflöde som hjälper dig medan spelet växer.

## Lärandemål

Efter kapitlet ska du kunna:

- förklara vad Git, repository, commit och remote betyder
- förstå varför Unity-projekt behöver en genomtänkt `.gitignore`
- skapa ett lokalt Git-repository för Skogssprånget
- göra tydliga commits efter fungerande ändringar
- publicera projektet till GitHub eller liknande tjänst
- undvika vanliga misstag med stora filer, Library-mappen och hemliga uppgifter

## Innan vi börjar

Du behöver projektet från föregående kapitel. Det ska innehålla:

- Unity-projektet Skogssprånget
- minst en spelbar scen
- scripts, prefabs, scener och projektinställningar
- en projektmapp som du kan hitta i filsystemet

Du behöver också ha Git installerat. Du kan använda Git via terminalen, GitHub Desktop, Visual Studio Code eller ett annat grafiskt verktyg. I kapitlet visar vi både begreppen och ett enkelt terminalbaserat arbetsflöde, eftersom kommandona gör det tydligt vad som händer.

## Huvudförklaring

### Vad Git gör

Git är ett versionshanteringssystem. Det betyder att Git kan spara historik över filer i ett projekt.

Tänk på Git som ett system för tydliga sparpunkter. Varje sparpunkt kallas en **commit**. En commit bör representera en meningsfull ändring, till exempel:

- lade till spelarens hopp
- skapade startmeny
- fixade checkpoint-bugg
- lade till poängtext i UI

En commit är inte samma sak som att bara spara en fil. När du sparar en fil skriver du ändringen till datorn. När du gör en commit säger du: “Den här samlingen ändringar hör ihop och är värd att komma ihåg.”

### Repository, commit och remote

Ett **repository**, ofta förkortat **repo**, är projektmappen tillsammans med Git-historiken.

En **commit** är en sparad ändringspunkt i repositoryt.

En **remote** är en kopia av repositoryt på en annan plats, ofta GitHub. Den lokala kopian finns på din dator. Remote-kopian finns online.

Ett vanligt arbetsflöde ser ut så här:

1. Du ändrar filer i Unity och i dina scripts.
2. Du testar att spelet fortfarande fungerar.
3. Du väljer vilka ändringar som ska ingå.
4. Du gör en commit med ett tydligt meddelande.
5. Du skickar upp ändringen till GitHub med push.

### Varför Unity-projekt behöver särskild hantering

Unity-projekt består av många filer. Vissa ska versionshanteras. Andra ska inte versionshanteras.

Filer och mappar som normalt ska ingå:

- `Assets/`
- `ProjectSettings/`
- `Packages/`
- `.gitignore`
- eventuella egna dokument, exempel och README-filer

Filer och mappar som normalt inte ska ingå:

- `Library/`
- `Temp/`
- `Logs/`
- `Obj/`
- `Build/`
- automatiskt skapade cachefiler

Den viktigaste regeln är denna: versionshantera projektets källfiler, inte Unitys tillfälliga arbetsmappar.

### `.gitignore`

En `.gitignore` är en textfil som talar om för Git vilka filer och mappar som ska ignoreras.

Skapa filen i Unity-projektets rotmapp, alltså samma nivå som `Assets`, `Packages` och `ProjectSettings`.

En enkel `.gitignore` för Unity kan börja så här:

```gitignore
[Ll]ibrary/
[Tt]emp/
[Oo]bj/
[Bb]uild/
[Bb]uilds/
[Ll]ogs/
[Uu]ser[Ss]ettings/

*.csproj
*.sln
*.user
*.pidb
*.booproj
*.svd
*.pdb
*.mdb

.DS_Store
Thumbs.db

.vscode/
.idea/
```

Det här är inte den enda möjliga varianten, men den räcker som pedagogisk start. I ett skarpt projekt bör du jämföra med en aktuell Unity-anpassad `.gitignore` från en pålitlig källa innan du publicerar projektet.

### Skapa ett lokalt repository

Stäng Unity innan du gör den allra första Git-initieringen. Det minskar risken att Unity håller på att skriva temporära filer samtidigt.

Öppna terminalen i projektets rotmapp. Mappen ska innehålla till exempel:

```text
Assets
Packages
ProjectSettings
```

Kör sedan:

```bash
git init
git status
```

`git init` skapar Git-historiken i projektmappen. `git status` visar vilka filer Git ser.

Om `.gitignore` fungerar ska du inte se `Library/` som en stor mängd filer som vill läggas till. Om du ändå ser `Library/`, kontrollera att `.gitignore` ligger på rätt plats och att mappnamnet stämmer.

### Gör första commiten

När `.gitignore` är på plats och `git status` ser rimlig ut kan du göra din första commit:

```bash
git add .
git commit -m "Initial Unity project"
```

`git add .` betyder att du förbereder aktuella ändringar för commit.

`git commit -m "Initial Unity project"` skapar en sparpunkt med meddelandet “Initial Unity project”.

Ett bra commit-meddelande är kort men tydligt. Skriv hellre:

```text
Add player jump and ground check
```

än:

```text
Fix stuff
```

### Arbeta i små steg

Från och med nu bör du göra commits efter fungerande steg. Ett bra mönster är:

1. Planera en liten ändring.
2. Gör ändringen.
3. Testa i Unity.
4. Kontrollera `git status`.
5. Gör commit.
6. Fortsätt med nästa ändring.

Exempel:

```bash
git status
git add Assets/Scripts/PlayerMovement.cs
git commit -m "Tune player movement speed"
```

Du behöver inte alltid lägga till alla filer. Ibland är det bättre att bara committa de filer som hör till en viss ändring.

### Publicera till GitHub

För att publicera projektet online skapar du först ett tomt repository på GitHub eller en liknande tjänst. Skapa det utan att lägga till extra README, licens eller `.gitignore` där, eftersom projektet redan finns lokalt.

När du har skapat repositoryt får du en remote-adress. Den kan se ut ungefär så här:

```text
https://github.com/anvandarnamn/skogsspranget.git
```

Koppla din lokala projektmapp till remote-repositoryt:

```bash
git remote add origin https://github.com/anvandarnamn/skogsspranget.git
git branch -M main
git push -u origin main
```

Efter detta finns projektet online. Nästa gång räcker det ofta med:

```bash
git push
```

När du vill hämta ändringar från remote använder du:

```bash
git pull
```

Om du arbetar ensam behöver du inte använda `pull` lika ofta, men det är bra att känna igen kommandot.

### GitHub Desktop som alternativ

Om terminalen känns ovan kan du använda GitHub Desktop. Principen är densamma:

1. Välj projektmappen som lokalt repository.
2. Kontrollera att `.gitignore` finns.
3. Skriv ett commit-meddelande.
4. Klicka på Commit.
5. Klicka på Publish repository eller Push origin.

Det viktiga är inte vilket verktyg du använder. Det viktiga är att du förstår arbetsflödet: ändra, testa, committa, pusha.

### Branches i korthet

En **branch** är en parallell arbetsgren. Du kan använda branches för att testa större ändringar utan att störa huvudversionen.

Exempel:

```bash
git switch -c test-new-enemy
```

Nu arbetar du på en ny branch. Om experimentet blir bra kan du slå ihop det senare. Om det blir rörigt kan du lämna det utan att huvudversionen påverkas.

I den här boken behöver du inte använda branches hela tiden. För en nybörjarvänlig soloprototyp räcker ofta `main` och tydliga commits. Men när du börjar göra större experiment är branches ett bra nästa steg.

## Exempel: Versionshantera Skogssprånget

Anta att din projektmapp heter:

```text
Skogsspranget
```

Mappstrukturen kan se ut så här:

```text
Skogsspranget/
├── Assets/
├── Packages/
├── ProjectSettings/
└── .gitignore
```

Du har precis avslutat kapitel 12 och spelet har startmeny, nivå och omstart.

Ett rimligt arbetsflöde är:

```bash
git status
git add .
git commit -m "Add menu and basic game loop"
git push
```

Sedan börjar du på nästa lilla förbättring, till exempel en ny checkpoint-effekt. När den fungerar:

```bash
git status
git add Assets/Scripts/Checkpoint.cs Assets/Scenes/Level01.unity
git commit -m "Improve checkpoint feedback"
git push
```

Lägg märke till att varje commit beskriver vad som ändrades på en spelmässig nivå. Det gör historiken användbar.

## När Git säger stopp: merge conflicts

En *merge conflict* uppstår när Git inte säkert kan avgöra vilken version av en fil som ska gälla. Det händer ofta när samma rad har ändrats på två olika håll, till exempel på två branches eller på två datorer.

För en ensam utvecklare är konflikter ovanligare än i ett team, men de kan ändå hända. Ett typiskt scenario är:

1. Du ändrar `PlayerMovement.cs` på din dator.
2. Du har också ändrat samma fil på en annan dator eller i en annan branch.
3. Du försöker köra `git pull` eller slå ihop en branch.
4. Git markerar filen som konfliktfylld.

### Hur en konflikt kan se ut

Git skriver in markeringar direkt i filen:

```csharp
<<<<<<< HEAD
[SerializeField] private float moveSpeed = 6f;
=======
[SerializeField] private float moveSpeed = 8f;
>>>>>>> feature/faster-player
```

Det här betyder:

- delen efter `<<<<<<< HEAD` är din nuvarande version
- delen efter `=======` är den andra versionen
- raden efter `>>>>>>>` visar var den andra ändringen kom ifrån

Git vet inte om spelaren ska ha hastigheten `6f` eller `8f`. Du måste välja.

### Så löser du konflikten säkert

Öppna filen och ta bort konfliktmarkeringarna. Välj den version som ska vara kvar, eller kombinera ändringarna om det behövs.

Exempel på löst fil:

```csharp
[SerializeField] private float moveSpeed = 7f;
```

Spara filen och kontrollera att koden fortfarande kompilerar i Unity. Kör sedan:

```bash
git status
git add Assets/Scripts/PlayerMovement.cs
git commit
```

Om konflikten uppstod vid en merge skapar Git ofta ett färdigt commit-meddelande. Läs det och spara commiten.

### Arbetsregel för Unity-projekt

Lös aldrig konflikter i blindo, särskilt inte i Unitys scenfiler, prefabfiler eller metadatafiler. De kan vara svårare att läsa än C#-filer.

En säker regel är:

- lös C#-konflikter manuellt när du förstår ändringarna
- undvik att två personer ändrar samma scen samtidigt
- gör små commits ofta
- testa projektet i Unity efter en löst konflikt
- använd `git status` för att se att arbetsytan är ren efteråt

### När du vill backa ur

Om du precis börjat en merge och inser att det blev fel kan du ofta avbryta med:

```bash
git merge --abort
```

Det tar dig tillbaka till läget före mergeförsöket. Använd det bara innan du har börjat lösa konflikten på riktigt.

## Vanliga misstag

- **Misstag: Att lägga upp `Library/` på GitHub.**
  - Varför det händer: `.gitignore` saknas, ligger på fel plats eller skapades efter att filerna redan lagts till.
  - Hur man undviker det: skapa `.gitignore` innan första commit och kontrollera `git status`.

- **Misstag: Att göra enorma commits med många olika ändringar.**
  - Varför det händer: man väntar för länge mellan commits.
  - Hur man undviker det: committa efter små fungerande steg.

- **Misstag: Att skriva otydliga commit-meddelanden.**
  - Varför det händer: man tänker att historiken inte spelar roll.
  - Hur man undviker det: skriv vad ändringen gör, till exempel `Add coin pickup sound`.

- **Misstag: Att versionshantera byggfiler i onödan.**
  - Varför det händer: build-mappar ligger i projektet och ignoreras inte.
  - Hur man undviker det: lägg builds i en ignorerad mapp eller utanför repositoryt.

- **Misstag: Att publicera hemliga nycklar eller privata filer.**
  - Varför det händer: man lägger till allt utan att granska.
  - Hur man undviker det: kontrollera alltid `git status` och publicera inte hemligheter i projektfiler.

## Övningar

### Övning 1: Skapa `.gitignore`

Skapa en `.gitignore` i Unity-projektets rotmapp. Kontrollera att `Library/`, `Temp/` och `Logs/` ignoreras.

Svara på frågan: Varför ska `Assets/` versionshanteras men inte `Library/`?

### Övning 2: Gör första commiten

Initiera Git i projektmappen och gör en första commit.

Använd commit-meddelandet:

```text
Initial Unity project
```

Kontrollera därefter historiken med:

```bash
git log --oneline
```

### Övning 3: Gör en liten ändring

Ändra en liten sak i projektet, till exempel spelarens hastighet eller texten på startknappen.

Testa spelet och gör sedan en ny commit med ett tydligt meddelande.

### Fördjupning

Skapa en branch för ett experiment, till exempel:

```bash
git switch -c experiment-double-jump
```

Testa en ändring utan att committa den direkt till `main`. Fundera på när en branch är värdefull och när den bara gör arbetsflödet mer komplicerat.

## Snabb sammanfattning

- Git sparar historik över projektets ändringar.
- Ett repository är projektet tillsammans med Git-historiken.
- En commit är en tydlig sparpunkt.
- GitHub kan användas som remote för att lagra projektet online.
- Unity-projekt behöver en `.gitignore` så att tillfälliga mappar inte versionshanteras.
- Bra commits är små, testade och tydligt beskrivna.
- För en soloprototyp räcker ett enkelt arbetsflöde långt: ändra, testa, committa och pusha.

## Quiz/reflektionsfrågor

1. Vad är skillnaden mellan att spara en fil och att göra en commit?
2. Varför ska `Library/` normalt inte läggas upp i Git?
3. Vad betyder remote i Git-sammanhang?
4. När kan det vara bättre att göra två små commits än en stor?
5. Vilket commit-meddelande är tydligast: `stuff` eller `Add checkpoint respawn`? Varför?

## Nästa steg

Nu har projektet både spelmekanik och ett tryggare arbetssätt. I nästa kapitel bygger vi, testar och paketerar prototypen. Då använder vi den stabila projektstrukturen och Git-historiken för att avsluta boken med en spelbar version av Skogssprånget.
