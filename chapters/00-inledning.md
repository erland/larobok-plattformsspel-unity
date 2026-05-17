# Inledning

Att skapa ett plattformsspel är ett bra sätt att lära sig programmering mer strukturerat. Du får snabbt se resultatet av din kod på skärmen, samtidigt som du möter många av de idéer som används i riktiga spelprojekt: objekt, rörelse, fysik, kollisioner, användargränssnitt, nivåer och versionshantering.

Den här boken hjälper dig bygga ett enkelt 2D-plattformsspel i Unity. Spelet har arbetsnamnet **Skogssprånget**. Under bokens gång kommer du att skapa en spelare som kan röra sig och hoppa, bygga en bana, lägga till hinder och fiender, samla objekt, visa poäng och till slut paketera projektet till en spelbar prototyp.

## Vem boken är för

Boken är skriven för dig som har experimenterat lite med programmering men inte känner att du lärt dig programmera på ett strukturerat sätt. Du behöver inte vara expert på C# eller Unity. Däremot hjälper det om du är nyfiken, vågar testa och kan tänka dig att felsöka steg för steg.

Du kommer att möta engelska begrepp som är vanliga i Unity, till exempel **GameObject**, **Component**, **Scene** och **Inspector**. När sådana begrepp dyker upp första gången får du en svensk förklaring.

## Hur boken är upplagd

Boken börjar med arbetsmiljön: Unity Hub, Unity Editor och hur ett projekt skapas. Därefter går vi igenom Unitys arbetsyta och de C#-grunder som behövs för att skriva dina första scripts.

Sedan bygger vi spelmekaniken i små steg:

- spelaren rör sig
- spelaren hoppar
- banan får plattformar
- kameran följer spelaren
- fiender och hinder läggs till
- poäng, hälsa och checkpoints ger spelet struktur
- menyer och build-inställningar gör prototypen mer komplett

Mot slutet går vi igenom hur projektet versionshanteras med Git och GitHub. Det är viktigt eftersom spelprojekt snabbt får många filer, och versionshantering gör det lättare att spara, backa, dela och fortsätta utveckla utan att tappa kontrollen.

## Så använder du boken

Det bästa sättet att läsa boken är att bygga samtidigt. Läs ett avsnitt, gör stegen i Unity och testa ofta. Om något inte fungerar är det inte ett misslyckande, utan en normal del av spelutveckling. Många professionella utvecklare ägnar mycket tid åt att undersöka varför något beter sig annorlunda än väntat.

Varje kapitel innehåller:

- vad kapitlet ska hjälpa dig förstå
- de viktigaste begreppen
- ett praktiskt moment i spelet
- vanliga misstag
- övningar
- kort sammanfattning
- kontrollfrågor eller reflektionsfrågor

## Vad boken inte försöker vara

Det här är inte en komplett referens till hela Unity. Den försöker inte heller täcka avancerad grafikprogrammering, nätverksspel, artificiell intelligens eller kommersiell publicering. Målet är i stället att ge dig en stabil grund: du ska förstå hur ett Unity-projekt hänger ihop och kunna bygga vidare på egen hand.

## Innan du börjar

Du behöver en dator som kan köra Unity, internetuppkoppling för att ladda ner verktyg och en vilja att arbeta metodiskt. Exakta installationsdetaljer kan ändras över tid, så om Unity Hub eller GitHub ser annorlunda ut än i boken bör du jämföra med aktuell officiell dokumentation.

I nästa kapitel installerar vi verktygen och skapar projektet som resten av boken bygger på.
