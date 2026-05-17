# Kapitel 1: Installera verktygen och skapa första projektet

## Varför detta kapitel finns

Innan vi kan programmera en spelare, bygga banor eller lägga till fiender behöver vi en fungerande arbetsmiljö. Det är lätt att vilja hoppa direkt till själva spelet, men ett tydligt projekt från början gör allt senare arbete enklare.

I det här kapitlet installerar du Unity, väljer en editor för C#-kod och skapar projektet för bokens exempelspel **Skogssprånget**.

## Lärandemål

Efter kapitlet ska du kunna:

- förklara skillnaden mellan Unity Hub och Unity Editor
- skapa ett nytt 2D-projekt i Unity
- känna igen projektets viktigaste mappar
- spara en första scen
- förstå varför projektstruktur spelar roll

## Innan vi börjar

Du behöver en dator med internetanslutning och möjlighet att installera program. Boken utgår från att du använder Unitys vanliga GameObject-baserade arbetsflöde.

Exakta knappar, namn och installationssteg kan förändras när Unity uppdateras. Om något ser annorlunda ut ska du följa samma princip: installera Unity Hub, installera en Unity Editor-version och skapa ett 2D-projekt.

## Huvudförklaring

### Unity Hub och Unity Editor

Unity består i praktiken av två delar som du kommer att använda ofta.

**Unity Hub** är programmet som hjälper dig hantera Unity-installationer och projekt. Här kan du installera olika Unity-versioner, skapa nya projekt och öppna gamla projekt.

**Unity Editor** är själva arbetsmiljön där du bygger spelet. Det är där du placerar objekt, ändrar inställningar, kopplar scripts och testar spelet.

En enkel jämförelse är att Unity Hub är som en startpanel, medan Unity Editor är verkstaden där du arbetar.

### Välj Unity-version

När du installerar Unity behöver du välja en editor-version. För en lärobok är det bäst att använda en stabil version med långsiktigt stöd om en sådan finns tillgänglig. Unitys rekommendationer och versionsnamn kan ändras, så kontrollera gärna aktuell information i Unity Hub eller på Unitys officiella webbplats.

För bokens syfte är det viktigaste inte exakt versionsnummer, utan att du använder en modern Unity-version som stöder 2D-projekt och C#-scripts.

### Installera kodredigerare

Unity-projekt programmeras vanligtvis i C#. Du behöver därför en kodredigerare. Vanliga val är Visual Studio eller Visual Studio Code.

En kodredigerare hjälper dig med:

- färgmarkering av kod
- förslag medan du skriver
- felmeddelanden
- navigering mellan filer

I början är det viktigaste att redigeraren öppnas korrekt när du dubbelklickar på ett script i Unity.

### Skapa projektet

Öppna Unity Hub och skapa ett nytt projekt. Välj en 2D-mall. Namnge projektet:

```text
Skogsspranget
```

Använd gärna ett projektnamn utan svenska tecken i filsystemet. Bokens titel använder **Skogssprånget**, men projektmappen får heta **Skogsspranget** för att undvika problem med verktyg, sökvägar och delning mellan olika operativsystem.

Placera projektet i en tydlig arbetsmapp, till exempel:

```text
UnityProjects/Skogsspranget
```

Undvik att lägga projektet direkt i en synkad molnmapp om du inte vet att det fungerar bra med Unity. Unity skapar många små filer, och vissa molntjänster kan ibland orsaka konflikter.

### Första gången projektet öppnas

När Unity Editor öppnas första gången kan det ta en stund. Unity bygger upp projektets interna filer och förbereder arbetsytan.

Du behöver inte förstå hela gränssnittet direkt. I nästa kapitel går vi igenom arbetsytan mer systematiskt. Just nu räcker det att veta att projektet är skapat och att du kan spara en scen.

### Skapa en startscen

En **Scene** är en avgränsad del av spelet. Det kan vara en nivå, en meny eller en testmiljö. Vårt första mål är att skapa en scen där spelet senare ska börja.

Skapa en mapp i projektet som heter:

```text
Scenes
```

Spara den nuvarande scenen som:

```text
MainScene
```

En enkel struktur i projektets `Assets`-mapp kan se ut så här:

```text
Assets/
  Scenes/
    MainScene.unity
```

Det här är ett litet steg, men det gör projektet mer organiserat från början.

## Exempel

När projektet är skapat har du ännu inget spel, men du har en arbetsplats där spelet kan växa. Tänk på projektet som en tom verkstad.

- Unity Hub öppnar verkstaden.
- Unity Editor är arbetsbänken.
- `Assets` är hyllan där du lägger material.
- `Scenes` är mappen där spelets nivåer och menyer sparas.
- Senare kommer `Scripts` att innehålla din C#-kod.

Redan nu kan du skapa följande grundstruktur:

```text
Assets/
  Scenes/
  Scripts/
  Sprites/
  Prefabs/
```

Du behöver inte fylla mapparna ännu. Poängen är att projektet får en tydlig form innan det blir större.

## Vanliga misstag

- **Misstag: Att skapa flera testprojekt och sedan glömma vilket som är rätt.**
  - Varför det händer: Det känns ofarligt att testa olika projekt i början.
  - Hur du undviker det: Bestäm en projektmapp och använd samma projekt genom hela boken.

- **Misstag: Att lägga projektet i en rörig eller svårhittad mapp.**
  - Varför det händer: Installationen görs snabbt utan plan.
  - Hur du undviker det: Skapa en tydlig huvudmapp, till exempel `UnityProjects`.

- **Misstag: Att oroa sig för hela Unity-gränssnittet direkt.**
  - Varför det händer: Unity visar många fönster och inställningar.
  - Hur du undviker det: Fokusera i början bara på att skapa, öppna och spara projektet.

- **Misstag: Att använda specialtecken i projektnamnet.**
  - Varför det händer: Det naturliga svenska namnet innehåller å, ä eller ö.
  - Hur du undviker det: Använd ett tekniskt projektnamn utan svenska tecken, till exempel `Skogsspranget`.

## Övningar

### Övning 1: Skapa projektet

Skapa ett nytt Unity-projekt med namnet `Skogsspranget`.

Kontrollera att:

- projektet öppnas i Unity Editor
- du kan se projektets `Assets`-mapp
- du kan spara scenen som `MainScene`
- scenen ligger i `Assets/Scenes`

### Övning 2: Skapa grundmappar

Skapa följande mappar under `Assets`:

```text
Scenes
Scripts
Sprites
Prefabs
```

Skriv kort i egna anteckningar vad du tror att varje mapp kommer användas till. Det gör inget om dina förklaringar är enkla.

### Fördjupning

Undersök var Unity-projektet ligger på din dator. Öppna projektmappen i filhanteraren och titta på vilka mappar Unity har skapat.

Ändra inte filer manuellt ännu. Målet är bara att se att ett Unity-projekt består av mer än det som syns inne i editorn.

## Snabb sammanfattning

- Unity Hub används för att installera Unity och öppna projekt.
- Unity Editor är arbetsmiljön där spelet byggs.
- Boken använder ett 2D-projekt med arbetsnamnet `Skogsspranget`.
- En tydlig mappstruktur gör projektet lättare att förstå.
- Första scenen sparas som `MainScene` i `Assets/Scenes`.

## Quiz/reflektionsfrågor

1. Vad är skillnaden mellan Unity Hub och Unity Editor?
2. Varför är det klokt att använda ett projektnamn utan svenska tecken i filsystemet?
3. Vad används en Scene till i Unity?
4. Varför kan det vara bra att skapa mappar som `Scripts`, `Sprites` och `Prefabs` redan i början?
5. Vad skulle kunna bli svårt om du skapade många olika testprojekt utan tydliga namn?

## Nästa steg

Nu har du ett Unity-projekt som resten av boken kan bygga vidare på. I nästa kapitel går vi igenom Unitys arbetsyta: vilka fönster som är viktigast, vad ett GameObject är och hur Components används för att ge objekt egenskaper och beteenden.
