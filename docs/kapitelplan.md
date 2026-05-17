# Kapitelplan

## Del 1: Grund och arbetsmiljö

### Kapitel 0: Inledning
- Syfte: Förklara bokens mål, målgrupp, arbetssätt och exempelprojekt.
- Läsarens förkunskaper: Viss datorvana och lite experimenterande med programmering.
- Nya huvudbegrepp: Unity, C#, spelprojekt.
- Praktiskt exempel/scenario: Exempelspelet Skogssprånget presenteras.
- Övning: Sätt upp en personlig projektmapp och skriv ner mål.
- Svårighetsgrad: Grundnivå.
- Bygger vidare på: Inga tidigare kapitel.

### Kapitel 1: Installera verktygen och skapa första projektet
- Syfte: Hjälpa läsaren installera rätt verktyg och skapa ett fungerande Unity-projekt.
- Läsarens förkunskaper: Grundläggande datorvana.
- Nya huvudbegrepp: Unity Hub, Unity Editor, projektmall.
- Praktiskt exempel/scenario: Skapa Unity-projektet för Skogssprånget.
- Övning: Skapa projektet, öppna scenen och spara första versionen.
- Svårighetsgrad: Grundnivå.
- Bygger vidare på: Inledningen.

### Kapitel 2: Förstå Unitys arbetsyta
- Syfte: Förklara de viktigaste vyerna och hur objekt organiseras.
- Läsarens förkunskaper: Har öppnat ett Unity-projekt.
- Nya huvudbegrepp: Scene, GameObject, Component.
- Praktiskt exempel/scenario: Skapa enkla objekt i startscenen.
- Övning: Bygg en enkel testscen.
- Svårighetsgrad: Grundnivå.
- Bygger vidare på: Kapitel 1.

### Kapitel 3: C# för Unity: grunderna du behöver
- Syfte: Ge den programmeringsgrund som behövs för de kommande kapitlen.
- Läsarens förkunskaper: Har testat programmering men behöver struktur.
- Nya huvudbegrepp: variabel, metod, klass.
- Praktiskt exempel/scenario: Skapa ett första script och koppla det till ett objekt.
- Övning: Ändra värden i Inspector och se effekten.
- Svårighetsgrad: Grundnivå.
- Bygger vidare på: Kapitel 2.

## Del 2: Spelarens grundmekanik

### Kapitel 4: Spelarens första rörelse
- Syfte: Skapa horisontell rörelse med fysik.
- Nya huvudbegrepp: input, Rigidbody2D, hastighet.
- Praktiskt exempel/scenario: Spelarfiguren kan gå åt vänster och höger.
- Övning: Justera rörelsehastighet.
- Svårighetsgrad: Grundnivå.
- Bygger vidare på: Kapitel 3.

### Kapitel 5: Hopp, gravitation och markkontroll
- Syfte: Skapa ett stabilt hopp och förstå vanliga rörelsebuggar.
- Nya huvudbegrepp: ground check, LayerMask, kraft.
- Praktiskt exempel/scenario: Spelaren kan hoppa när den står på marken.
- Övning: Finjustera hoppkänslan.
- Svårighetsgrad: Grundnivå.
- Bygger vidare på: Kapitel 4.

### Kapitel 6: Bygg en enkel bana
- Syfte: Skapa en första spelbar plattformsmiljö.
- Nya huvudbegrepp: collider, prefab, nivådesign.
- Praktiskt exempel/scenario: En enkel bana med start, hinder och mål.
- Övning: Bygg en egen kort bana.
- Svårighetsgrad: Grundnivå.
- Bygger vidare på: Kapitel 5.

### Kapitel 7: Kamera som följer spelaren
- Syfte: Göra spelet mer spelbart med följande kamera.
- Nya huvudbegrepp: kamera, follow target, spelvy.
- Praktiskt exempel/scenario: Kameran följer spelarfiguren.
- Övning: Testa olika kamerainställningar.
- Svårighetsgrad: Grundnivå.
- Bygger vidare på: Kapitel 6.

## Del 3: Spelregler och interaktion

### Kapitel 8: Fiender och hinder
- Syfte: Introducera enkel fara och interaktion.
- Nya huvudbegrepp: fiendelogik, patrullering, kollision.
- Praktiskt exempel/scenario: En fiende rör sig mellan två punkter.
- Övning: Skapa ett hinder som skadar spelaren.
- Svårighetsgrad: Grundnivå.
- Bygger vidare på: Kapitel 7.

### Kapitel 9: Hälsa, liv och checkpoints
- Syfte: Ge spelet struktur när spelaren misslyckas.
- Nya huvudbegrepp: hälsa, respawn, checkpoint.
- Praktiskt exempel/scenario: Spelaren återvänder till senaste checkpoint.
- Övning: Lägg till fler checkpoints.
- Svårighetsgrad: Grundnivå.
- Bygger vidare på: Kapitel 8.

### Kapitel 10: Samla objekt och visa poäng
- Syfte: Skapa belöningar och enkel UI-feedback.
- Nya huvudbegrepp: collectible, score, UI text.
- Praktiskt exempel/scenario: Spelaren samlar mynt och ser poäng.
- Övning: Lägg till bonusobjekt.
- Svårighetsgrad: Grundnivå.
- Bygger vidare på: Kapitel 9.

### Kapitel 11: Animationer och spelkänsla
- Syfte: Göra prototypen tydligare och roligare att spela.
- Nya huvudbegrepp: Animator, animation state, feedback.
- Praktiskt exempel/scenario: Gå-, hopp- och idle-animationer.
- Övning: Lägg till enkel ljudeffekt eller visuell feedback.
- Svårighetsgrad: Grundnivå.
- Bygger vidare på: Kapitel 10.

## Del 4: Färdig prototyp och arbetsflöde

### Kapitel 12: Menyer, nivåbyte och spel-loop
- Syfte: Knyta ihop spelets början, slut och omstart.
- Nya huvudbegrepp: scene loading, game over, restart.
- Praktiskt exempel/scenario: Startmeny och game over-skärm.
- Övning: Skapa en enkel vinstskärm.
- Svårighetsgrad: Grundnivå.
- Bygger vidare på: Kapitel 11.

### Kapitel 13: Versionshantera Unity-projekt med Git och GitHub
- Syfte: Lära läsaren spara, dela och skydda sitt projekt.
- Nya huvudbegrepp: repository, commit, .gitignore.
- Praktiskt exempel/scenario: Lägg Skogssprånget i ett GitHub-repository.
- Övning: Gör en ändring, committa och synka.
- Svårighetsgrad: Grundnivå.
- Bygger vidare på: Kapitel 12.

### Kapitel 14: Bygg, testa och paketera prototypen
- Syfte: Skapa en spelbar build och sammanfatta nästa steg.
- Nya huvudbegrepp: build, testning, polish.
- Praktiskt exempel/scenario: Exportera en spelbar version av Skogssprånget.
- Övning: Gör en egen förbättringslista.
- Svårighetsgrad: Grundnivå.
- Bygger vidare på: Kapitel 13.

## Progressionskontroll
- Begrepp introduceras i rätt ordning: miljö först, Unity-arbetsyta sedan C#, därefter rörelse, spelregler och arbetsflöde.
- För svåra hopp: Git placeras sent för att inte störa den första Unity-inlärningen, men projektet förbereds tidigt för ordning.
- Repetitionstillfällen: C#-begrepp repeteras när scripts byggs ut.
- Slutprojekt eller sammanfattande moment: Boken slutar med en spelbar prototyp och en förbättringsplan.
