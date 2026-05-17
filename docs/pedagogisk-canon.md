# Pedagogisk canon

## Språk
Svenska. Engelska begrepp används när de är etablerade i Unity, exempelvis GameObject, Component, Scene, Rigidbody2D, commit och repository. Första gången ett sådant begrepp används ges en kort svensk förklaring.

## Svårighetsgrad
Grundnivå. Läsaren har experimenterat med programmering men behöver struktur.

## Läsarprofil
Läsaren vill bygga något konkret och lära sig programmering genom ett spelprojekt. Hen behöver tydliga steg, många kontrollpunkter och förklaringar av varför saker görs.

## Ton
Vänlig, praktisk och uppmuntrande. Hellre små fungerande steg än stora teoretiska block.

## Exempelprojekt
**Skogssprånget** är ett 2D-plattformsspel där spelaren:
- styr en figur i en skogsmiljö
- hoppar mellan plattformar
- undviker fiender och hinder
- samlar objekt
- når ett mål

## Versions- och faktaval
Boken ska använda Unitys klassiska GameObject-baserade arbetsflöde. Exakta Unity-versioner, installationssteg och GitHub-gränssnitt bör verifieras mot aktuell officiell dokumentation innan slutexport.

## Introducerade koncept hittills
- Unity: spelmotor och editor för att skapa spel.
- Unity Hub: verktyg för att installera och hantera Unity-versioner och projekt.
- Unity Editor: själva arbetsmiljön där spelet byggs.
- Projektmall: startpunkt för ett Unity-projekt.
- Scene: avgränsad del av spelet, till exempel nivå, meny eller testmiljö.
- GameObject: Unitys grundobjekt i en scen.
- Component: del som ger ett GameObject egenskaper eller beteende.
- Transform: Component som styr position, rotation och storlek.

- Script: C#-fil som innehåller beteende som kan kopplas till ett GameObject.
- Klass: behållare för kod som hör ihop.
- Metod: namngivet block med instruktioner.
- Variabel: namn på ett värde som programmet kan använda.
- Start: Unity-metod som körs en gång när scriptet startar.
- Update: Unity-metod som körs varje bildruta.
- Debug.Log: instruktion som skriver meddelanden till Unity Console.

## Progressionsanteckning efter kapitel 2
Läsaren har nu skapat/öppnat projektet, känner till de viktigaste editorfönstren och kan resonera om Scene, GameObject och Component.

## Progressionsanteckning efter kapitel 3
Läsaren har skapat enkla C#-scripts, kopplat scripts till GameObjects och introducerats till klass, metod, variabel, `Start()`, `Update()`, publika Inspector-värden och `Debug.Log()`. Kapitel 4 kan därför använda dessa begrepp för spelarens första horisontella rörelse, men bör repetera dem i praktisk kontext.


- Input: spelarens styrsignaler, till exempel knapptryckningar för vänster och höger.
- Rigidbody2D: Unity-komponent som kopplar ett 2D-objekt till fysiksystemet.
- Hastighet: hur snabbt något rör sig i en riktning.
- FixedUpdate: Unity-metod som används i regelbundna fysiksteg.
- Vector2: tvådimensionellt värde med X och Y, ofta använt för positioner och hastigheter.

## Progressionsanteckning efter kapitel 4
Läsaren har skapat en enkel spelare med `Rigidbody2D`, `BoxCollider2D` och scriptet `PlayerMovement`. Läsaren har sett hur input läses i `Update()` och hur horisontell hastighet sätts i `FixedUpdate()`. Kapitel 5 kan bygga vidare med hopp, gravitation och markkontroll, men bör repetera varför Y-hastigheten behöver bevaras.


- Ground check: kontroll som avgör om spelaren står på marken.
- LayerMask: urval av Unity-lager som ett script kan kontrollera mot.
- Physics2D.OverlapCircle: metod som kontrollerar om en cirkel överlappar en 2D-collider.
- Gravity Scale: inställning på `Rigidbody2D` som påverkar hur starkt gravitationen verkar.

## Progressionsanteckning efter kapitel 5
Läsaren har byggt vidare på `PlayerMovement` med hopp, markkontroll och justering av gravitation. Läsaren har introducerats till `GroundCheck`, `LayerMask`, `Physics2D.OverlapCircle` och `Gravity Scale`. Kapitel 6 kan använda spelarens rörelse och hopp för att bygga en enkel testbana med plattformar och kollisionsytor.


- Nivådesign: att forma banan så att spelaren lär sig, utmanas och kan använda spelets mekanik.
- Tilemap: rutnätsbaserat system där banor byggs med återanvändbara tiles.
- Tile: liten byggbit som kan målas ut i en Tilemap.
- Tile Palette: Unity-fönster där tiles väljs och målas ut.
- Tilemap Collider 2D: Component som ger kollision till tiles på en Tilemap.
- Fysiklager: den del av banan som spelaren kan kollidera med.

## Progressionsanteckning efter kapitel 6
Läsaren har byggt en första enkel bana, förstått skillnaden mellan visuella objekt och fysiklager, använt Tilemap eller enkla rektanglar för prototypnivåer och kopplat banans mark till spelarens `groundLayer`. Kapitel 7 kan därför fokusera på kamerans roll utan att samtidigt introducera grundläggande banbygge.


- Kamera: GameObject med Camera-component som bestämmer vad spelaren ser i Game-vyn.
- Follow target: Transform-objekt som kameran följer, i detta projekt spelarens Transform.
- Game-vy: Unity-vyn som visar kamerans bild av spelet.
- LateUpdate: Unity-metod som körs efter vanliga Update och används här för kameraföljning.
- Offset: positionsskillnad som gör att kameran kan ligga lite framför eller ovanför spelaren.

## Progressionsanteckning efter kapitel 7
Läsaren har skapat en enkel följande kamera med `CameraFollow`, kopplat spelarens Transform som `target` och justerat `offset` och `smoothSpeed`. Kapitel 8 kan därför utgå från att spelaren, banan och kameran fungerar tillsammans när fiender och hinder introduceras.

- Fiendelogik: regler som styr hur en fiende beter sig.
- Patrullering: rörelse där en fiende går mellan två punkter.
- Trigger: collider-läge som upptäcker kontakt utan att blockera rörelse.
- OnTriggerEnter2D: Unity-metod som reagerar på kontakt med en 2D-trigger.
- Tag: etikett på ett GameObject, exempelvis `Player`.

## Progressionsanteckning efter kapitel 8
Läsaren har skapat en patrullerande fiende med `EnemyPatrol`, använt patrullpunkter och återanvänt `DamageZone` för både fiender och hinder. Läsaren har introducerats till triggers, taggar och `OnTriggerEnter2D`. Kapitel 9 kan bygga vidare genom att låta farlig kontakt påverka spelarens hälsa, liv och respawn.

- Hälsa: tal som beskriver hur många träffar spelaren tål.
- Respawn: att flytta tillbaka spelaren till en säker position.
- Checkpoint: plats som sparar spelarens nya respawn-position.
- PlayerHealth: script som hanterar spelarens hälsa, skada och respawn.
- Damage Amount: värde som anger hur mycket skada ett farligt objekt gör.
- GetComponent: Unity-metod som hämtar en component eller ett script från ett GameObject.
- FallZone: trigger under banan som används för att återställa spelaren vid fall.

## Progressionsanteckning efter kapitel 9
Läsaren har byggt ett enkelt hälsosystem med `PlayerHealth`, uppdaterat `DamageZone` så att faror kan skada spelaren och skapat checkpoints som uppdaterar respawn-positionen. Kapitel 10 kan nu introducera belöningar och UI-feedback genom samlingsobjekt, poäng och textvisning. Console används fortfarande som tillfällig återkoppling tills UI byggs ut.



- Samlingsobjekt: objekt som spelaren kan plocka upp för poäng eller belöning.
- Poäng: tal som visar spelarens framsteg eller belöning.
- UI: användargränssnitt som visar information på skärmen.
- Canvas: Unity-objekt som fungerar som yta för UI.
- TextMeshPro: textsystem som används för UI-text.
- ScoreManager: script som håller reda på poängen och uppdaterar UI.
- Destroy: Unity-metod som tar bort ett GameObject.
- Singleton: enkel åtkomstpunkt till en gemensam instans av ett script.

## Progressionsanteckning efter kapitel 10
Läsaren har skapat samlingsobjekt med `Collectible`, använt triggerkontakt för att ge poäng, byggt `ScoreManager` och kopplat poäng till en TextMeshPro-baserad UI-text. Kapitel 11 kan bygga vidare genom att ge spelaren tydligare visuell och auditiv återkoppling med animationer och spelkänsla.


- Animation clip: en enskild animation, till exempel idle, run eller jump.
- Animator: Unity-komponent som spelar upp animationer på ett GameObject.
- Animator Controller: fil som beskriver animation states, parametrar och övergångar.
- Animation state: ett läge i Animator, till exempel `Idle`, `Run` eller `Jump`.
- Spelkänsla: hur tydligt, responsivt och roligt spelet känns när spelaren styr det.
- AudioSource: komponent som spelar upp ljud.
- AudioClip: ljudfil som kan spelas upp.
- PlayOneShot: metod som spelar upp ett ljud en gång.

## Progressionsanteckning efter kapitel 11
Läsaren har kopplat en Animator till spelaren, skapat grundläggande animation states för idle, run och jump samt styrt dem från `PlayerController` med parametrarna `Speed` och `IsGrounded`. Läsaren har också fått en första praktisk introduktion till spelkänsla genom enkel ljud- eller visuell feedback. Kapitel 12 kan därför bygga vidare genom att skapa en tydligare spel-loop med startmeny, game over och nivåbyte.



- Spel-loop: spelets övergripande flöde från meny till spel, resultat och omstart.
- SceneManager: Unity-klass som används för att ladda scener.
- Build Settings: listan över scener som ska ingå när spelet körs eller byggs.
- UI-knapp: interaktiv knapp som kan kopplas till en public method i ett script.
- Time.timeScale: värde som kan pausa eller återställa spelets tid.

## Progressionsanteckning efter kapitel 12
Läsaren har skapat en enkel startmeny, kopplat UI-knappar till script, laddat scener med `SceneManager`, lagt till ett mål i nivån och skapat ett enkelt vinst-/omstartsflöde. Kapitel 13 kan därför introducera Git och GitHub med ett projekt som nu har flera scener, scripts och inställningar som behöver skyddas genom versionshantering.


## Progressionsanteckning efter kapitel 13

Läsaren har nu introducerats till Git som praktiskt skyddsnät för Unity-projektet. Nya begrepp är repository, commit, remote, `.gitignore`, push, pull och branch. Kapitel 14 kan anta att läsaren har ett versionshanterat projekt och kan göra en commit innan större build- och paketeringssteg.

## Versionshanteringsprincip för resten av boken

När större ändringar görs i Skogssprånget bör kapitlen uppmuntra läsaren att:
- testa spelet först
- kontrollera ändrade filer
- göra små commits med tydliga meddelanden
- undvika att versionshantera tillfälliga Unity-mappar och builds


- Build: fristående version av spelet som kan köras utanför Unity Editor.
- Build Settings: Unity-fönster där scener och målplattform väljs inför build.
- Testplan: kort dokument som beskriver vad som ska testas och vilket resultat som förväntas.
- Spelbar loop: central spelcykel där spelaren gör något, spelet svarar och spelaren försöker igen.
- Git-tag: markering av en specifik version i Git-historiken.

## Progressionsanteckning efter kapitel 14
Läsaren har nu byggt, testat och paketerat Skogssprånget som en spelbar prototyp. Alla planerade kapitel finns som utkast. Nästa naturliga steg är helhetsgranskning, faktaverifiering mot aktuell Unity-/GitHub-dokumentation och exportförberedelse.

## Revideringsnoteringar

### Kapitel 14
Kapitel 14 ska fungera som bokens professionella avslut. Tonen ska vara uppmuntrande men konkret: läsaren har inte byggt ett färdigt kommersiellt spel, men har byggt en spelbar prototyp och lärt sig ett arbetsflöde.

Kapitlet betonar:
- skillnaden mellan Unity Editor och färdig build
- pre-build-checklista
- testplan
- paketering av buildmapp
- README till testare
- feedbackhantering
- versionsnummer och Git-tagg
- vidareutveckling efter boken

Avslutningen ska kännas som en övergång från övningsprojekt till eget spelprojekt.


## Kompletteringar efter första manusgranskning

- Kapitel 5 har kompletterats med coyote time och jump buffering som frivilliga förbättringar för mer förlåtande hoppkänsla.
- Kapitel 13 har kompletterats med ett konkret exempel på merge conflict och ett säkert arbetsflöde för att lösa konflikter.
- Dessa begrepp behandlas som praktiska fortsättningsbegrepp: de förstärker tidigare kunskap utan att ändra bokens grundnivå.
