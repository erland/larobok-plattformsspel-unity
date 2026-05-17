# Kapitel 14: Bygg, testa och paketera prototypen

## Varför detta kapitel finns

Nu har Skogssprånget blivit mer än en samling övningar. Du har byggt spelarrörelse, hopp, bana, kamera, fiender, checkpoints, poäng, animationer, menyer och versionshantering. Det betyder att projektet har blivit en spelbar prototyp.

Men ett spel är inte färdigt bara för att det fungerar när du trycker på Play i Unity Editor. En prototyp behöver testas som ett riktigt spel. Den behöver byggas till en fristående version som andra kan starta utan att öppna Unity. Den behöver också paketeras så att testaren förstår vad filen är, hur spelet startas och vad du vill ha feedback på.

Det här kapitlet handlar därför om övergången från “det fungerar på min dator” till “någon annan kan prova spelet”.

## Lärandemål

Efter kapitlet ska du kunna:

- förklara skillnaden mellan att spela i Unity Editor och att köra en färdig build
- kontrollera att rätt scener finns med i Build Settings
- skapa en enkel testplan för en spelbar prototyp
- göra en build för en vald plattform
- paketera spelet så att det går att dela med en testspelare
- samla in användbar feedback utan att göra testningen onödigt komplicerad
- planera nästa steg efter bokens prototyp

## Innan vi börjar

Du behöver ha ett Unity-projekt som går att spela från början till slut, även om det är kort. Det behöver inte vara perfekt. En prototyp får vara ofärdig, enkel och lite ojämn. Det viktiga är att det finns en spelbar kärna:

- spelaren kan starta spelet
- spelaren kan röra sig och hoppa
- banan har ett tydligt mål eller slut
- det finns någon form av risk, hinder eller utmaning
- spelet kan startas om efter game over eller när nivån är klar

Du behöver också ha sparat projektet i Git innan du gör större ändringar inför en build. På så sätt kan du gå tillbaka om du råkar ändra något som förstör projektet.

## Editor och build är inte samma sak

När du spelar i Unity Editor körs spelet i en utvecklingsmiljö. Unity hjälper dig med mycket:

- du kan se felmeddelanden direkt i Console
- du kan pausa spelet och inspektera objekt
- du kan ändra värden i Inspector medan spelet körs
- du kör spelet inuti Unitys egen miljö

En build är annorlunda. Då paketerar Unity spelet som ett fristående program för en viss plattform, till exempel Windows, macOS eller WebGL. Spelaren ser inte Unity Editor. Spelaren ser bara spelet.

Det betyder att vissa problem kan dyka upp först i en build:

- en scen saknas i Build Settings
- en filväg fungerar i editorn men inte i builden
- spelets fönsterstorlek känns fel
- UI hamnar annorlunda än väntat
- ljudvolymen känns för hög eller låg
- spelet startar i fel scen

En viktig vana är därför:

> Testa inte bara i Unity Editor. Testa också den färdiga builden.

## Steg 1: Gör en säker versionspunkt

Innan du börjar ändra inställningar inför builden bör du göra en tydlig commit.

Öppna terminalen i projektmappen och kör:

```bash
git status
git add .
git commit -m "Prepare prototype build"
```

Om Git säger att det inte finns något att committa är det okej. Då är projektet redan sparat.

Det viktiga är att du vet var du är. En build-förberedelse kan innebära små ändringar i scener, menyer, inställningar och UI. Då är det skönt att ha en säker punkt att återvända till.

## Steg 2: Kontrollera spelets scenflöde

Innan du bygger spelet behöver du veta vilka scener som ska ingå. Ett enkelt projekt kan till exempel ha:

| Scen | Syfte |
|---|---|
| `MainMenu` | Startmeny |
| `Level01` | Första spelbara banan |
| `GameOver` | Visas när spelaren förlorar |
| `WinScreen` | Visas när spelaren klarar nivån |

Alla scener måste inte vara separata. Ett litet projekt kan ha meny och spel i samma scen. Men oavsett struktur behöver du veta vilken scen som ska starta först.

### Kontrollera Build Settings

Gör så här:

1. Öppna Unity.
2. Välj `File > Build Settings`.
3. Kontrollera listan under `Scenes In Build`.
4. Lägg till saknade scener med `Add Open Scenes`.
5. Dra scenerna i rätt ordning.

Den scen som ligger överst i listan blir normalt den scen som startar först.

För Skogssprånget rekommenderar vi att startmenyn ligger först:

```text
0 MainMenu
1 Level01
2 GameOver
3 WinScreen
```

Om spelet startar direkt i banan är det inte fel, men då ska det vara ett medvetet val.

## Steg 3: Gör en enkel pre-build-checklista

En pre-build-checklista är en kort kontroll du gör varje gång du ska skapa en version som någon annan ska testa.

Använd gärna den här listan:

- Startmenyn fungerar.
- Spelet går att starta från menyn.
- Spelaren kan röra sig och hoppa.
- Kameran följer spelaren.
- Fiender eller hinder fungerar.
- Checkpoints eller respawn fungerar.
- Poäng eller samlarobjekt fungerar om de används.
- Game over fungerar.
- Restart fungerar.
- Ljudvolymen är rimlig.
- UI syns tydligt.
- Det finns inget testobjekt kvar i banan som inte ska vara där.
- Unity Console visar inga återkommande fel under normalt spel.

Listan är enkel, men den skyddar dig mot de vanligaste misstagen.

## Steg 4: Skapa en testplan

En testplan behöver inte vara avancerad. Den ska bara göra testningen mindre slumpmässig.

Här är en enkel testplan för Skogssprånget:

| Test | Vad du gör | Förväntat resultat |
|---|---|---|
| Starta spelet | Öppna builden | Startmenyn visas |
| Starta nivå | Tryck på startknappen | Level01 laddas |
| Rörelse | Gå vänster och höger | Spelaren rör sig kontrollerat |
| Hopp | Hoppa på plan mark | Spelaren hoppar och landar |
| Plattformar | Hoppa mellan plattformar | Kollisionsytor fungerar |
| Fiende | Rör vid fiende | Spelaren tar skada eller respawnar |
| Checkpoint | Aktivera checkpoint och misslyckas | Spelaren återvänder till checkpoint |
| Samlarobjekt | Plocka upp coin | Poäng ändras |
| Game over | Förlora alla liv | Game over visas |
| Restart | Starta om efter game over | Spelet börjar om korrekt |

Testplanen gör att du inte bara “spelar lite och hoppas”. Du kontrollerar spelets viktigaste delar.

## Steg 5: Välj build-plattform

Första gången du bygger spelet bör du välja en plattform som är enkel att testa själv.

Vanliga val är:

| Plattform | Passar för | Kommentar |
|---|---|---|
| Windows | Test på egen eller andras Windows-datorer | Bra standardval om du använder Windows |
| macOS | Test på Mac | Kräver att testaren använder macOS |
| WebGL | Dela via webbläsare | Praktiskt men kan ge fler plattformsspecifika problem |

För en första prototyp rekommenderas Windows eller macOS beroende på din egen dator. WebGL kan vara ett bra senare steg om du vill dela spelet lättare via en webbsida.

## Steg 6: Gör en build

Öppna `File > Build Settings`.

Gör sedan så här:

1. Välj plattform.
2. Klicka på `Switch Platform` om Unity behöver byta.
3. Kontrollera att rätt scener finns i `Scenes In Build`.
4. Klicka på `Build`.
5. Skapa en ny mapp, till exempel `Builds/Skogsspranget-v0.1`.
6. Låt Unity skapa builden där.

Lägg inte buildfilerna direkt i projektets huvudmapp. Det blir lätt rörigt, och du riskerar att blanda ihop källprojekt och exporterade filer.

En rimlig mappstruktur är:

```text
Skogssprånget/
├── Unity-projektet/
└── Builds/
    └── Skogsspranget-v0.1/
```

Om du har `Builds/` inuti Unity-projektet bör den normalt ignoreras av Git, eftersom buildfiler ofta är stora och kan återskapas.

## Steg 7: Testa builden som en spelare

När builden är klar ska du inte bara konstatera att Unity inte visade något fel. Du ska starta spelet från den färdiga buildmappen.

Gör så här:

1. Stäng eller minimera Unity.
2. Öppna buildmappen.
3. Starta spelet från den skapade programfilen.
4. Spela igenom från start till slut.
5. Anteckna allt som känns konstigt.

Det är viktigt att spela som en ny spelare, inte som utvecklaren som redan vet hur allt fungerar.

Fråga dig:

- Förstår jag hur spelet startar?
- Förstår jag vad målet är?
- Märker jag när jag tar skada?
- Märker jag när jag vinner eller förlorar?
- Känns rörelsen rättvis?
- Finns det något läge där spelet fastnar?

## Steg 8: Paketera prototypen

När builden fungerar behöver du paketera den så att någon annan kan testa.

En enkel paketmapp kan se ut så här:

```text
Skogsspranget-v0.1/
├── Skogsspranget.exe
├── Skogsspranget_Data/
└── README.txt
```

På andra plattformar ser filerna annorlunda ut, men principen är densamma: alla filer som Unity skapar för builden måste följa med.

### Skriv en README till testaren

Skapa en enkel `README.txt` i buildmappen:

```text
Skogssprånget v0.1

Det här är en tidig prototyp av ett 2D-plattformsspel.

Så startar du:
1. Packa upp zip-filen.
2. Starta Skogsspranget.exe.
3. Spela med piltangenter eller A/D och Space för hopp.

Saker att testa:
- Känns rörelsen tydlig?
- Förstår du vad målet är?
- Är banan för lätt, för svår eller lagom?
- Hittar du buggar där spelaren fastnar?
- Fungerar restart efter game over?

Skicka gärna feedback med:
- vad som hände
- vad du förväntade dig
- om du kan upprepa problemet
```

Det här gör testningen mycket bättre. Testaren vet vad du vill få reda på.

## Steg 9: Samla in feedback

Bra feedback är konkret. Då kan du agera på den.

Mindre användbar feedback:

> Spelet känns konstigt.

Mer användbar feedback:

> Hoppet känns lite för tungt. Jag missade andra plattformen tre gånger fast jag tryckte hopp nära kanten.

När du ber om feedback kan du ställa några få frågor:

1. Förstod du hur du skulle spela?
2. Var fastnade du?
3. Vad kändes roligast?
4. Vad kändes mest frustrerande?
5. Var det något som inte fungerade?

Be inte om tjugo saker samtidigt. Då blir testningen jobbig och svaren sämre.

## Steg 10: Skapa en liten förbättringsplan

Efter testningen ska du inte försöka fixa allt direkt. Dela upp feedbacken i tre grupper:

| Grupp | Exempel | Åtgärd |
|---|---|---|
| Buggar | Spelet fastnar efter game over | Fixa först |
| Spelkänsla | Hoppet känns för tungt | Justera och testa igen |
| Nya idéer | Lägg till boss, fler världar, butik | Spara till senare |

En prototyp blir bättre när du gör små förbättringar och testar igen.

Ett bra arbetssätt är:

1. Välj 3–5 viktiga förbättringar.
2. Gör ändringarna.
3. Testa i Unity Editor.
4. Gör en ny build.
5. Låt någon testa igen.

Det är så spel växer fram: inte genom en perfekt plan, utan genom upprepade små förbättringar.

## Versionsnummer för prototyper

Det är praktiskt att ge varje testversion ett enkelt versionsnummer.

Exempel:

| Version | Betydelse |
|---|---|
| `v0.1` | Första spelbara prototypen |
| `v0.2` | Förbättrad rörelse och bana |
| `v0.3` | Mer polish, ljud och bättre UI |
| `v1.0` | Första versionen du betraktar som färdig |

Du behöver inte följa en avancerad standard. Poängen är att du och testaren vet vilken version ni pratar om.

Gör gärna en Git-commit efter varje testbar version:

```bash
git add .
git commit -m "Build prototype v0.1"
```

Du kan också skapa en tagg:

```bash
git tag v0.1
```

En tagg är en markering i Git-historiken. Den säger: “här fanns version 0.1”.

## Vanliga misstag

### Misstag: Spelet startar i fel scen

Varför det händer:

Scenerna ligger i fel ordning i Build Settings.

Hur du undviker det:

Kontrollera att startmenyn eller första spelbara scenen ligger överst i `Scenes In Build`.

### Misstag: En scen fungerar i editorn men saknas i builden

Varför det händer:

Scenen finns i projektet men är inte tillagd i Build Settings.

Hur du undviker det:

Öppna varje viktig scen och använd `Add Open Scenes`, eller dra in scenerna manuellt i listan.

### Misstag: Du skickar bara `.exe`-filen

Varför det händer:

Det är lätt att tro att programfilen är hela spelet.

Hur du undviker det:

Skicka hela buildmappen. Unity-buildar består ofta av flera filer och mappar.

### Misstag: Du testar bara själv

Varför det händer:

Som utvecklare vet du redan hur spelet fungerar.

Hur du undviker det:

Låt minst en annan person testa utan att du förklarar för mycket först. Titta på vad personen försöker göra.

### Misstag: Du försöker fixa all feedback samtidigt

Varför det händer:

Feedback kan kännas som en lång att-göra-lista.

Hur du undviker det:

Välj först buggar och sådant som hindrar spelaren från att förstå eller slutföra prototypen.

## Övningar

### Övning 1: Gör en pre-build-kontroll

Gå igenom checklistan i kapitlet och markera varje punkt som fungerar i ditt projekt.

Skriv också ned minst tre saker som behöver kontrolleras igen innan du delar spelet.

### Övning 2: Skapa en testplan

Skapa en egen testplan med minst åtta testfall för Skogssprånget.

Varje testfall ska ha:

- vad testaren gör
- vad som förväntas hända
- vad du ska anteckna om något går fel

### Övning 3: Gör din första build

Skapa en build av prototypen. Starta den utanför Unity Editor och spela igenom hela spelet.

Anteckna:

- vad som fungerade
- vad som inte fungerade
- vad som kändes annorlunda jämfört med Unity Editor

### Övning 4: Skriv en README till testaren

Skriv en `README.txt` för din prototyp.

Den ska innehålla:

- spelets namn och version
- hur spelet startas
- vilka kontroller som används
- vad du vill att testaren ska fokusera på
- hur testaren kan beskriva buggar

### Fördjupning: Skapa en v0.1-release

Skapa en Git-commit och en Git-tagg för din första testbara version.

Exempel:

```bash
git add .
git commit -m "Create first playable prototype"
git tag v0.1
```

Skriv sedan en kort release-notering:

```text
Skogssprånget v0.1

Första spelbara prototypen:
- startmeny
- en kort bana
- rörelse och hopp
- checkpoints
- samlarobjekt
- game over och restart

Kända problem:
- hoppet behöver finjusteras
- vissa plattformar kan kännas för svåra
```

## Snabb sammanfattning

- En build är en fristående version av spelet, inte samma sak som att spela i Unity Editor.
- Rätt scener måste finnas i Build Settings.
- Testa alltid builden utanför Unity.
- En enkel testplan hjälper dig hitta problem mer systematiskt.
- Skicka hela buildmappen, inte bara en enskild programfil.
- En README gör det lättare för andra att testa.
- Feedback bör sorteras i buggar, spelkänsla och framtida idéer.
- Små förbättringar och nya builds är ett realistiskt arbetssätt för spelutveckling.

## Quiz/reflektionsfrågor

1. Varför räcker det inte att bara testa spelet i Unity Editor?
2. Vad händer om en scen inte finns med i Build Settings?
3. Varför bör du skicka med en README till testaren?
4. Vad är skillnaden mellan en bugg och en förbättring av spelkänsla?
5. Varför är det bättre att välja några få förbättringar efter testning än att försöka fixa allt direkt?
6. Vad är nyttan med att skapa en Git-tagg för `v0.1`?

## Nästa steg

Du har nu byggt en komplett första prototyp av Skogssprånget. Den är inte ett färdigt kommersiellt spel, men den innehåller hela kedjan från projektstart till spelbar build:

- Unity-projekt
- C#-scripts
- spelarrörelse
- hopp och fysik
- bana
- kamera
- fiender och hinder
- hälsa och checkpoints
- samlarobjekt och UI
- animationer och spelkänsla
- menyer och spel-loop
- Git och GitHub
- build, testning och paketering

Det viktigaste du kan göra nu är att fortsätta arbeta i små cykler:

1. förbättra en sak
2. testa
3. committa
4. bygg en ny version
5. samla feedback

Det är så du går från övningsprojekt till eget spelprojekt.

### Förslag på vidareutveckling

Här är några naturliga nästa steg:

- lägg till fler banor
- skapa tydligare mål i varje nivå
- förbättra animationer och ljud
- skapa fler fiendetyper
- lägg till pause-meny
- skapa sparad progression
- förbättra kontrollerna med coyote time och jump buffering
- testa spelet på fler datorer
- publicera en liten WebGL-version för testare

Du behöver inte göra allt. Välj en riktning som känns rolig. Ett bra nästa projekt är inte nödvändigtvis större. Det är tydligare, mer genomarbetat och mer medvetet.

Grattis. Du har inte bara följt en serie Unity-steg. Du har byggt en spelbar prototyp och lärt dig ett arbetsflöde som kan användas för många framtida spel.
