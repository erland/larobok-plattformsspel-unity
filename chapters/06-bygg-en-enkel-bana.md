# Kapitel 6: Bygg en enkel bana

## Varför detta kapitel finns

Nu kan spelaren röra sig åt sidan och hoppa. Det betyder att vi har en spelare som kan testas, men ännu ingen riktig bana att spela på. Ett plattformsspel blir begripligt först när spelaren får hinder, avstånd, höjdskillnader och ytor som går att landa på.

I det här kapitlet bygger vi en första enkel bana till **Skogssprånget**. Målet är inte att skapa en vacker slutlig nivå. Målet är att skapa en testbar nivå där rörelse, hopp och markkontroll går att prova på riktigt.

Vi introducerar tre huvudbegrepp:

- nivådesign i liten skala
- Tilemap
- kollisionsytor för bana

## Lärandemål

Efter kapitlet ska läsaren kunna:

- skapa en enkel 2D-bana som går att testa med spelaren
- förstå skillnaden mellan visuella objekt och kollisionsobjekt
- använda Tilemap som ett praktiskt sätt att bygga plattformar
- lägga till kollision på mark och plattformar
- tänka på banans form som ett verktyg för att testa spelmekanik

## Innan vi börjar

Vi fortsätter från kapitel 5. Där har `Player`:

- `Rigidbody2D`
- `BoxCollider2D`
- `PlayerMovement`
- horisontell rörelse
- hopp
- markkontroll

Om spelaren kan hoppa men faller genom världen saknas troligen en markyta med collider. Om spelaren står stilla på marken men inte kan hoppa saknas troligen rätt inställning för `groundLayer` eller `groundCheck`.

I det här kapitlet bygger vi banan på ett sätt som gör att markkontrollen från kapitel 5 får något att kontrollera mot.

## Huvudförklaring

### En bana är mer än dekoration

När man börjar bygga spel är det lätt att tänka på banan som det som syns: mark, träd, bakgrund och plattformar. För spelmotorn är det viktigare att veta vad spelaren kan kollidera med.

Därför kan vi tänka på varje del av banan i två lager:

- **visuellt lager:** det spelaren ser
- **fysiklager:** det spelaren kan stå på, slå i eller stoppas av

I enkla prototyper kan samma objekt ha båda rollerna. En grå rektangel kan både synas som plattform och ha en `BoxCollider2D`. Senare kan vi byta ut rektanglarna mot snyggare grafik utan att ändra hela spelmekaniken.

### Två sätt att bygga en första bana

Det finns två vanliga sätt att snabbt bygga en bana i Unity:

1. använda enkla GameObjects med `SpriteRenderer` och `BoxCollider2D`
2. använda Unitys 2D Tilemap-system

Båda fungerar. För den här boken använder vi Tilemap som huvudväg, eftersom det passar plattformsspel bra och gör det lättare att bygga många markbitar. Samtidigt börjar vi med en mycket enkel variant så att verktyget inte blir viktigare än förståelsen.

### Vad är en Tilemap?

En **Tilemap** är ett rutnät där du kan måla ut små byggbitar, så kallade tiles. En tile kan till exempel vara en bit mark, ett hörn, en gräsruta eller en sten.

För ett plattformsspel är Tilemap användbart eftersom du kan:

- bygga om banan snabbt
- återanvända samma byggbitar
- skapa plattformar och väggar utan att placera varje objekt för hand
- lägga till kollision på hela kartan

Tänk på Tilemap som ett digitalt rutpapper där varje ruta kan få en spelbricka.

## Exempel: skapa första testbanan

I det här exemplet skapar vi en enkel testbana med mark, några plattformar och ett startområde. Banan ska vara tillräckligt enkel för att hitta fel i rörelse och hopp.

### Steg 1: skapa en ny scen eller använd testscenen

Om du redan har en scen från tidigare kapitel kan du fortsätta i den. Annars kan du skapa en ny scen:

1. Gå till `File > New Scene`.
2. Spara scenen i mappen `Assets/Scenes`.
3. Ge scenen namnet `Level_01`.

Om mappen `Scenes` inte finns, skapa den först i Project-vyn.

En tydlig projektstruktur kan till exempel vara:

```text
Assets/
├── Scenes/
│   └── Level_01.unity
├── Scripts/
│   └── PlayerMovement.cs
├── Sprites/
├── Tiles/
└── Tilemaps/
```

Det viktiga är inte exakt vilka mappar du har, utan att du kan hitta dina filer senare.

### Steg 2: skapa en Grid och Tilemap

I Hierarchy:

1. Högerklicka i Hierarchy.
2. Välj `2D Object > Tilemap > Rectangular`.
3. Unity skapar vanligtvis ett `Grid`-objekt med ett underobjekt för Tilemap.
4. Byt namn på Tilemap-objektet till `GroundTilemap`.

Du bör nu ha något i stil med:

```text
Grid
└── GroundTilemap
```

`Grid` bestämmer rutnätets struktur. `GroundTilemap` innehåller de tiles du målar ut.

### Steg 3: skapa enkla tiles

För att använda Tilemap behöver du tiles. Om du redan har ett spritepaket kan du använda det. För en tidig prototyp går det också bra att använda enkla färgade rutor.

Ett enkelt arbetssätt är:

1. Lägg en enkel mark-sprite i `Assets/Sprites`.
2. Markera spriten.
3. Kontrollera att den är importerad som `Sprite (2D and UI)`.
4. Skapa en mapp `Assets/Tiles`.
5. Dra spriten till Tile Palette eller skapa en tile från spriten när Unity frågar.

Om Tile Palette inte är öppen:

1. Gå till `Window > 2D > Tile Palette`.
2. Skapa en ny palette, till exempel `ForestPrototypePalette`.
3. Spara paletten i en lämplig mapp, till exempel `Assets/Tilemaps`.

Det kan kännas som många klick första gången. Poängen är att Unity behöver veta vilka små byggbitar du vill kunna måla med.

### Steg 4: måla en första markyta

Välj din tile i Tile Palette och måla ut en enkel markyta i Scene-vyn.

Börja med något mycket enkelt:

```text
XXXXXXXXXXXXXXXXXXXX
```

Varje `X` motsvarar en markruta. Lägg spelaren ovanför marken och tryck Play. Om banan ännu saknar collider kommer spelaren fortfarande att falla igenom. Det fixar vi i nästa steg.

### Steg 5: lägg till kollision på Tilemap

Markera `GroundTilemap` och lägg till komponenten:

```text
Tilemap Collider 2D
```

Nu får varje målad tile en kollisionsyta. Testa spelet igen. Spelaren bör landa på marken om:

- spelaren har `Rigidbody2D`
- spelaren har `BoxCollider2D`
- marken har `Tilemap Collider 2D`
- marken ligger på det lager som spelarens `groundLayer` kontrollerar mot

Om spelaren faller igenom, kontrollera först att du verkligen har målat tiles på den Tilemap som har collider.

### Steg 6: sätt rätt Layer för marken

I kapitel 5 använde vi ett `LayerMask`-fält, ofta kallat `groundLayer`. Det betyder att marken måste ligga på ett lager som scriptet känner igen.

Gör så här:

1. Markera `GroundTilemap`.
2. Öppna Layer-menyn högst upp i Inspector.
3. Skapa eller välj ett lager som heter `Ground`.
4. Sätt `GroundTilemap` till lagret `Ground`.
5. Markera `Player`.
6. Kontrollera att `groundLayer` i `PlayerMovement` är satt till `Ground`.

Nu kan markkontrollen hitta marken.

### Steg 7: bygg en liten testsekvens

En bra första bana ska testa en sak i taget. Bygg inte en stor nivå direkt. Skapa i stället en kort testsekvens:

```text
Start      Litet hopp       Högre hopp        Säker landning
XXXXX      XXXXX            XXX               XXXXXXXXX
```

I Unity kan det till exempel bli:

- en lång startplattform
- ett kort mellanrum
- en något högre plattform
- en längre yta där spelaren kan landa

Testa banan ofta. Efter varje ändring, tryck Play och känn efter:

- Går det att hoppa över mellanrummet?
- Är hoppet för högt eller för lågt?
- Fastnar spelaren i kanter?
- Känns rörelsen begriplig?
- Finns det plats att stanna och tänka?

### Steg 8: justera banan efter spelaren

I början är det bättre att justera banan efter spelarens nuvarande rörelse än att ändra rörelsekoden hela tiden.

Om spelaren inte klarar ett hopp kan du ändra:

- avståndet mellan plattformarna
- höjdskillnaden
- plattformens bredd
- spelarens `jumpForce`
- spelarens `moveSpeed`
- `Gravity Scale` på spelarens `Rigidbody2D`

Ändra helst bara en sak i taget. Annars blir det svårt att veta vad som faktiskt förbättrade eller försämrade spelkänslan.

## Alternativ: snabb prototyp med enkla rektanglar

Om Tilemap känns rörigt första gången kan du tillfälligt bygga banan med enkla GameObjects.

Skapa ett markobjekt:

1. Högerklicka i Hierarchy.
2. Välj `2D Object > Sprite > Square`.
3. Byt namn till `Ground`.
4. Skala objektet brett, till exempel X = 12 och Y = 1.
5. Lägg till `BoxCollider2D` om det inte redan finns.
6. Sätt objektets Layer till `Ground`.

Det här är ett fullt rimligt sätt att prototypa. Tilemap blir mest värdefullt när du vill bygga större banor med många upprepade markbitar.

## Vanliga misstag

- **Misstag: spelaren faller genom marken.**
  - Varför det händer: marken saknar collider, spelaren saknar collider, eller marken ligger inte där den ser ut att ligga.
  - Hur man undviker det: kontrollera både `BoxCollider2D` på spelaren och `Tilemap Collider 2D` eller `BoxCollider2D` på marken.

- **Misstag: spelaren kan stå på marken men inte hoppa.**
  - Varför det händer: marken har collider men ligger inte på rätt Layer för `groundLayer`.
  - Hur man undviker det: sätt markens Layer till `Ground` och kontrollera spelarens `groundLayer` i Inspector.

- **Misstag: banan blir för svår direkt.**
  - Varför det händer: man bygger med sin önskade slutnivå i huvudet i stället för att testa grundmekaniken.
  - Hur man undviker det: börja med breda plattformar, korta hopp och tydliga landningsytor.

- **Misstag: visuella objekt och kollisionsobjekt blandas ihop.**
  - Varför det händer: något ser ut som mark men saknar fysikkollision.
  - Hur man undviker det: fråga alltid: “Ska spelaren kunna stå på det här?” Om ja, behövs collider eller Tilemap-collider.

- **Misstag: små kanter stoppar spelaren.**
  - Varför det händer: colliders möts på ett sätt som skapar hakiga övergångar.
  - Hur man undviker det: håll prototypbanan enkel först. Mer avancerad collideroptimering kan göras senare.

## Övningar

### Övning 1: skapa en startplattform

Skapa en enkel startplattform som spelaren kan stå på när scenen startar.

Kontrollera att:

- spelaren inte faller genom marken
- spelaren kan gå vänster och höger
- spelaren kan hoppa från marken
- spelaren inte kan hoppa oändligt i luften

### Övning 2: bygg tre hopp

Bygg en kort bana med tre hopp:

1. ett mycket enkelt hopp
2. ett lite längre hopp
3. ett hopp till en något högre plattform

Skriv ner vilket hopp som känns bäst och varför.

### Övning 3: justera en parameter

Välj en parameter och ändra den:

- `moveSpeed`
- `jumpForce`
- `Gravity Scale`
- avståndet mellan plattformar
- höjdskillnaden mellan plattformar

Ändra bara en sak. Testa spelet igen och skriv ner vad som blev bättre eller sämre.

### Fördjupning

Skapa två olika varianter av samma bana:

- en lätt version
- en svårare version

Skillnaden får bara vara banans form, inte spelarens kod. Jämför hur mycket nivådesignen påverkar spelkänslan.

## Snabb sammanfattning

- En bana består både av det spelaren ser och det spelaren kan kollidera med.
- Tilemap gör det lättare att bygga 2D-banor av återanvändbara rutor.
- `Tilemap Collider 2D` ger kollision till målade tiles.
- Markens Layer måste stämma med spelarens `groundLayer`.
- En bra testbana börjar enkelt och testar en spelmekanik i taget.
- Nivådesign och spelarkontroll påverkar varandra.

## Quiz/reflektionsfrågor

1. Vad är skillnaden mellan ett visuellt lager och ett fysiklager?
2. Varför är Tilemap praktiskt i ett 2D-plattformsspel?
3. Vad behöver marken ha för att spelaren inte ska falla igenom?
4. Varför måste markens Layer stämma med spelarens `groundLayer`?
5. Varför är det klokt att bygga en enkel testbana innan man bygger en stor nivå?

## Nästa steg

Nu har spelaren en första bana att röra sig i. Nästa problem är att kameran ännu inte nödvändigtvis följer spelaren på ett bra sätt. I nästa kapitel bygger vi en kamera som följer spelaren och gör nivån mer spelbar.
