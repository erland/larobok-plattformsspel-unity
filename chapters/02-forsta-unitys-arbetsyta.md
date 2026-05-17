# Kapitel 2: Förstå Unitys arbetsyta

## Varför detta kapitel finns

När Unity öppnas för första gången kan arbetsytan kännas överfull. Det finns flera fönster, många knappar och ord som inte alltid förklarar sig själva. Men du behöver inte kunna allt på en gång.

I det här kapitlet fokuserar vi på de delar av Unity Editor som du kommer använda i nästan varje kapitel: **Scene**, **GameObject** och **Component**. När du förstår de tre begreppen blir Unity mycket lättare att navigera.

## Lärandemål

Efter kapitlet ska du kunna:

- känna igen de viktigaste fönstren i Unity Editor
- förklara vad en Scene är
- förklara vad ett GameObject är
- förstå hur Components ger objekt egenskaper och beteenden
- skapa och organisera enkla objekt i en testscen

## Innan vi börjar

Du ska ha skapat projektet `Skogsspranget` och sparat scenen `MainScene` i `Assets/Scenes`.

I kapitel 1 såg du att Unity Hub används för att öppna projektet och att Unity Editor är själva arbetsmiljön. Nu ska vi börja använda editorn mer medvetet.

## Huvudförklaring

### Arbetsytan i Unity Editor

Unity Editor består av flera fönster som samarbetar. De kan flyttas runt, så din layout kan se lite annorlunda ut än någon annans. Det viktiga är att förstå vad varje del används till.

De viktigaste fönstren just nu är:

- **Scene view**: där du bygger och placerar objekt.
- **Game view**: där du ser vad spelaren kommer se när spelet körs.
- **Hierarchy**: en lista över alla objekt i den öppna scenen.
- **Inspector**: visar och ändrar inställningar för det objekt du har markerat.
- **Project**: visar filer och mappar i projektets `Assets`-mapp.
- **Console**: visar felmeddelanden, varningar och utskrifter från kod.

Tänk på Unity som en verkstad. Scene view är arbetsbordet, Hierarchy är inventarielistan, Inspector är verktygspanelen för det valda objektet och Project är förrådet där materialet ligger.

### Scene: platsen där spelet byggs

En **Scene** är en avgränsad del av spelet. I ett färdigt spel kan en Scene vara en meny, en nivå eller en testsida.

I vårt projekt börjar vi med `MainScene`. Den kommer först vara en testscen där vi provar grunderna. Senare växer den till den första spelbara banan i **Skogssprånget**.

Det är vanligt att ha flera scener i större spel, men i början räcker det med en.

### GameObject: Unitys grundobjekt

Ett **GameObject** är Unitys grundläggande objekt i en Scene. Nästan allt du placerar i en Unity-scen är ett GameObject:

- en spelare
- en plattform
- en kamera
- ett ljus
- en fiende
- ett mynt
- en osynlig kontrollpunkt

Ett GameObject är i sig ganska tomt. Det viktiga är vilka delar det har kopplade till sig.

### Component: delar som ger objekt egenskaper

En **Component** är en del som sitter på ett GameObject och ger objektet en egenskap eller ett beteende.

Ett GameObject kan till exempel ha:

- en **Transform**, som bestämmer position, rotation och storlek
- en **Sprite Renderer**, som gör att objektet syns som en 2D-bild
- en **Rigidbody2D**, som låter objektet påverkas av fysik
- en **Collider2D**, som gör att objektet kan krocka med andra objekt
- ett eget C#-script, som beskriver objektets beteende

Alla GameObjects har en Transform. Det är därför varje objekt kan placeras någonstans i scenen.

Ett bra sätt att tänka är:

```text
GameObject = behållare
Component = delar som ger behållaren funktion
```

En spelare är alltså inte “magiskt” en spelare. Den är ett GameObject med rätt Components: något som syns, något som kolliderar, något som kan röra sig och senare ett script som styr beteendet.

### Inspector: där du ändrar det markerade objektet

När du markerar ett objekt i Hierarchy eller Scene view visas objektets Components i Inspector.

Det är här du ändrar värden, till exempel:

- objektets namn
- position
- storlek
- vilken sprite som ska visas
- om en Component är aktiv eller inte

Inspector blir extra viktig när vi börjar programmera. Många värden i våra scripts kommer kunna ändras direkt där, utan att vi behöver skriva om koden.

### Project-vyn: projektets filer

Project-vyn visar filer under `Assets`. Här ligger till exempel scener, scripts, sprites, ljud och prefabs.

Det är viktigt att skilja på Hierarchy och Project:

| Fönster | Visar | Exempel |
|---|---|---|
| Hierarchy | Objekt som finns i den öppna scenen | Player, Main Camera, Ground |
| Project | Filer som finns i projektet | MainScene.unity, Player.cs, grass.png |

Ett objekt i scenen är alltså inte samma sak som en fil i projektmappen. Senare kommer vi använda **Prefabs** för att spara återanvändbara objekt som filer, men det väntar vi med tills grunden sitter.

### Scene view och Game view

**Scene view** är där du bygger. Du kan flytta runt kameran i editorn, zooma och markera objekt.

**Game view** visar spelet från kamerans perspektiv. När du trycker på Play är det Game view som visar vad spelaren faktiskt ser.

Ett vanligt nybörjarmisstag är att bygga något i Scene view och sedan undra varför det inte syns i Game view. Ofta beror det på att kameran inte tittar på rätt plats, eller att objektet ligger utanför kamerans synfält.

## Exempel

Nu ska vi skapa en enkel testscen för **Skogssprånget**. Målet är inte att skapa grafik eller spelmekanik ännu, utan att se hur Scene, GameObject och Component hänger ihop.

### Skapa ett enkelt markobjekt

Skapa ett nytt 2D-objekt i scenen, till exempel en enkel sprite eller ett tomt objekt beroende på vilka alternativ din Unity-version visar. Namnge objektet:

```text
Ground
```

Markera objektet och titta i Inspector. Du bör se en Transform. Om objektet har en visuell Component, till exempel Sprite Renderer, visas den också där.

Sätt gärna objektets Scale så att det blir bredare:

```text
X: 8
Y: 1
Z: 1
```

Nu har du ett enkelt objekt som senare kan bli marken spelaren står på.

### Skapa ett enkelt spelarobjekt

Skapa ett till objekt och namnge det:

```text
Player
```

Placera det ovanför marken i Scene view. I det här läget behöver Player inte kunna röra sig. Det viktiga är att du ser objektet i Hierarchy och kan ändra dess värden i Inspector.

Du har nu två GameObjects:

```text
MainScene
  Main Camera
  Ground
  Player
```

Det är början på scenens struktur.

### Ordna objekt med namn

Bra namn gör Unity-projekt lättare att förstå. Det är bättre att ha objekt som heter `Player` och `Ground` än `Square`, `Circle` eller `GameObject`.

När projektet växer blir Hierarchy snabbt full av objekt. Tydliga namn är ett enkelt sätt att undvika förvirring.

## Vanliga misstag

- **Misstag: Att försöka förstå alla Unity-fönster samtidigt.**
  - Varför det händer: Editorn visar mycket information direkt.
  - Hur du undviker det: Fokusera först på Scene view, Game view, Hierarchy, Inspector och Project.

- **Misstag: Att blanda ihop Hierarchy och Project.**
  - Varför det händer: Båda visar listor med namn.
  - Hur du undviker det: Kom ihåg att Hierarchy visar objekt i scenen, medan Project visar filer i projektet.

- **Misstag: Att tro att ett GameObject alltid syns i spelet.**
  - Varför det händer: Namnet “objekt” låter som något visuellt.
  - Hur du undviker det: Kontrollera om objektet har en visuell Component, till exempel Sprite Renderer.

- **Misstag: Att ändra värden utan att veta vilket objekt som är markerat.**
  - Varför det händer: Inspector visar alltid det markerade objektet.
  - Hur du undviker det: Titta i Hierarchy innan du ändrar något och kontrollera objektets namn överst i Inspector.

## Övningar

### Övning 1: Identifiera Unitys viktigaste fönster

Öppna `MainScene` och hitta följande fönster:

- Scene view
- Game view
- Hierarchy
- Inspector
- Project
- Console

Skriv en mening om vad varje fönster används till.

### Övning 2: Skapa en enkel testscen

Skapa två objekt i scenen:

```text
Ground
Player
```

Placera `Player` ovanför `Ground`. Ändra storlek på `Ground` så att den liknar en enkel plattform.

Kontrollera sedan att båda objekten syns i Hierarchy.

### Övning 3: Undersök Components

Markera `Player` och titta i Inspector.

Besvara:

1. Vilka Components finns på objektet?
2. Vilken Component styr objektets position?
3. Vad händer om du ändrar värdena för Position i Transform?

### Fördjupning

Skapa ett tomt GameObject och namnge det:

```text
LevelRoot
```

Dra `Ground` under `LevelRoot` i Hierarchy om din Unity-version tillåter det. Det gör `Ground` till ett barnobjekt. Du behöver inte använda den här strukturen resten av boken ännu, men det ger en första känsla för att objekt kan organiseras hierarkiskt.

## Snabb sammanfattning

- Scene view används för att bygga och placera objekt.
- Game view visar vad spelaren ser genom kameran.
- Hierarchy visar objekten i den öppna scenen.
- Project visar projektets filer i `Assets`.
- Inspector visar inställningar för det markerade objektet.
- Ett GameObject är en behållare i scenen.
- Components ger GameObjects egenskaper och beteenden.
- Transform finns på alla GameObjects och styr position, rotation och storlek.

## Quiz/reflektionsfrågor

1. Vad är skillnaden mellan Scene view och Game view?
2. Vad är ett GameObject?
3. Varför är Components viktiga?
4. Varför finns Transform på alla GameObjects?
5. Vad är skillnaden mellan Hierarchy och Project?
6. Varför är tydliga objektnamn viktiga i ett Unity-projekt?

## Nästa steg

Nu kan du hitta i Unitys arbetsyta och förstå hur objekt byggs upp av Components. I nästa kapitel börjar vi med C# för Unity. Då skapar vi vårt första script och kopplar kod till ett GameObject.
