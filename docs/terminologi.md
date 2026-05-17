# Terminologi

| Begrepp | Svensk förklaring | Första kapitel |
|---|---|---|
| Unity | Spelmotor och editor som används för att skapa spelet. | Inledning |
| Unity Hub | Program för att installera Unity-versioner och skapa/öppna projekt. | Kapitel 1 |
| Unity Editor | Arbetsmiljön där scener, objekt och inställningar hanteras. | Kapitel 1 |
| GameObject | Grundläggande objekt i Unitys scener. | Kapitel 2 |
| Component | Del som ger ett GameObject egenskaper eller beteende. | Kapitel 2 |
| Scene | En nivå, meny, testmiljö eller annan avgränsad del av spelet. | Kapitel 2 |
| Script | C#-fil som innehåller kod för beteende i spelet. | Kapitel 3 |
| Repository | Versionshanterad projektmapp i Git. | Kapitel 13 |
| Commit | Sparad ändringspunkt i Git. | Kapitel 13 |
| Transform | Component som finns på alla GameObjects och styr position, rotation och storlek. | Kapitel 2 |

| Klass | Behållare för kod som hör ihop, ofta ett Unity-beteende i den här boken. | Kapitel 3 |
| Metod | Namngivet block med instruktioner som kan köras. | Kapitel 3 |
| Variabel | Namn på ett värde som programmet kan använda. | Kapitel 3 |
| Start | Unity-metod som körs en gång när scriptet startar. | Kapitel 3 |
| Update | Unity-metod som körs varje bildruta. | Kapitel 3 |
| Debug.Log | C#-instruktion i Unity som skriver meddelanden till Console. | Kapitel 3 |
| Input | Spelarens styrsignal, till exempel vänster, höger eller hopp. | Kapitel 4 |
| Rigidbody2D | Component som låter ett 2D-objekt påverkas av Unitys fysiksystem. | Kapitel 4 |
| Hastighet | Hur snabbt ett objekt rör sig i en riktning. | Kapitel 4 |
| FixedUpdate | Unity-metod som körs i fysiksteg och ofta används för fysikrörelse. | Kapitel 4 |
| Vector2 | Värde med X och Y som används för tvådimensionella positioner och hastigheter. | Kapitel 4 |
| BoxCollider2D | Component som ger ett 2D-objekt en kollisionsform. | Kapitel 4 |
| Ground check | Kontroll som avgör om spelaren står på marken. | Kapitel 5 |
| LayerMask | Urval av Unity-lager som ett script kan kontrollera mot. | Kapitel 5 |
| Physics2D.OverlapCircle | Metod som kontrollerar om en cirkel överlappar en 2D-collider. | Kapitel 5 |
| Gravity Scale | Inställning på Rigidbody2D som påverkar hur starkt gravitationen verkar. | Kapitel 5 |
| Nivådesign | Att forma banan så att spelaren lär sig, utmanas och kan använda spelets mekanik. | Kapitel 6 |
| Tilemap | Rutnätsbaserat system där banor kan byggas med återanvändbara tiles. | Kapitel 6 |
| Tile Palette | Unity-fönster där tiles väljs och målas ut på en Tilemap. | Kapitel 6 |
| Tilemap Collider 2D | Component som ger kollision till tiles på en Tilemap. | Kapitel 6 |
| Fysiklager | Den del av banan som spelaren kan kollidera med, till exempel mark och väggar. | Kapitel 6 |

| Kamera | Objekt och komponent som bestämmer vad spelaren ser i Game-vyn. | Kapitel 7 |
| Follow target | Objektets Transform som kameran följer. | Kapitel 7 |
| Game-vy | Unity-vyn som visar vad kameran ser när spelet körs. | Kapitel 7 |
| LateUpdate | Unity-metod som körs efter Update och passar när kameran ska följa spelaren. | Kapitel 7 |
| Offset | Positionsskillnad mellan två objekt, till exempel mellan kameran och spelaren. | Kapitel 7 |
| Vector3.Lerp | Metod som flyttar ett Vector3-värde gradvis mot ett annat. | Kapitel 7 |
| Fiendelogik | Regler som styr hur en fiende beter sig i spelet. | Kapitel 8 |
| Patrullering | Rörelsemönster där en fiende rör sig mellan två eller flera punkter. | Kapitel 8 |
| Trigger | Collider-läge som upptäcker kontakt utan att fungera som en fysisk vägg. | Kapitel 8 |
| OnTriggerEnter2D | Unity-metod som körs när en 2D-trigger får kontakt med en annan collider. | Kapitel 8 |
| Tag | Etikett på ett GameObject som kan användas för att känna igen objekt i kod. | Kapitel 8 |
| Vector2.MoveTowards | Metod som flyttar ett Vector2-värde stegvis mot ett mål. | Kapitel 8 |
| Hälsa | Tal som beskriver hur många träffar spelaren tål innan respawn eller annan konsekvens. | Kapitel 9 |
| Respawn | Att flytta tillbaka spelaren till en säker position efter att hälsan tagit slut eller spelaren fallit. | Kapitel 9 |
| Checkpoint | Plats som uppdaterar spelarens respawn-position. | Kapitel 9 |
| PlayerHealth | Script som hanterar spelarens hälsa, skada och respawn. | Kapitel 9 |
| Damage Amount | Värde som anger hur mycket skada ett farligt objekt gör. | Kapitel 9 |
| GetComponent | Unity-metod som hämtar en component eller ett script från ett GameObject. | Kapitel 9 |
| FallZone | Trigger under banan som skadar eller återställer spelaren när hen faller. | Kapitel 9 |


| Samlingsobjekt | Objekt som spelaren kan plocka upp för att få poäng eller annan belöning. | Kapitel 10 |
| Poäng | Tal som visar spelarens framsteg, belöning eller resultat. | Kapitel 10 |
| UI | Användargränssnitt som visar information på skärmen. | Kapitel 10 |
| Canvas | Unity-objekt som fungerar som yta för UI-element. | Kapitel 10 |
| TextMeshPro | Textsystem i Unity som ofta används för tydlig och flexibel UI-text. | Kapitel 10 |
| ScoreManager | Script som håller reda på poäng och uppdaterar poängtexten. | Kapitel 10 |
| Destroy | Unity-metod som tar bort ett GameObject från scenen. | Kapitel 10 |
| Singleton | Mönster där en gemensam instans av ett script kan nås från andra scripts. | Kapitel 10 |


| Animation clip | En enskild animation, till exempel idle, run eller jump. | Kapitel 11 |
| Animator | Unity-komponent som spelar upp animationer på ett GameObject. | Kapitel 11 |
| Animator Controller | Fil som beskriver animation states, parametrar och övergångar. | Kapitel 11 |
| Animation state | Ett läge i Animator, till exempel Idle, Run eller Jump. | Kapitel 11 |
| Spelkänsla | Hur tydligt, responsivt och roligt spelet känns när spelaren styr det. | Kapitel 11 |
| AudioSource | Component som kan spela upp ljud i Unity. | Kapitel 11 |
| AudioClip | Ljudfil som kan spelas upp av en AudioSource. | Kapitel 11 |
| PlayOneShot | Metod som spelar upp ett ljud en gång utan att stoppa andra ljud. | Kapitel 11 |

| Spel-loop | Spelets övergripande flöde från start till spel, resultat och omstart. | Kapitel 12 |
| SceneManager | Unity-klass som används för att ladda och byta scener. | Kapitel 12 |
| Build Settings | Inställning där projektets scener listas för körning och bygge. | Kapitel 12 |
| UI-knapp | Interaktiv knapp i spelets gränssnitt som kan anropa en public method. | Kapitel 12 |
| Time.timeScale | Värde som styr hur snabbt speltiden går och kan användas för paus. | Kapitel 12 |


| Git | Versionshanteringssystem som sparar ändringshistorik för projektet. | Kapitel 13 |
| Remote | Kopia av ett repository på en annan plats, ofta GitHub. | Kapitel 13 |
| .gitignore | Fil som anger vilka filer och mappar Git ska ignorera. | Kapitel 13 |
| Branch | Parallell arbetsgren i Git som kan användas för experiment. | Kapitel 13 |
| Push | Skicka lokala commits till en remote. | Kapitel 13 |
| Pull | Hämta ändringar från en remote till den lokala projektmappen. | Kapitel 13 |

| Build | Fristående version av spelet som kan köras utanför Unity Editor. | Kapitel 14 |
| Build Settings | Unity-fönster där scener och målplattform väljs inför build. | Kapitel 14 |
| Testplan | Kort dokument med testfall och förväntade resultat för prototypen. | Kapitel 14 |
| Spelbar loop | Spelets centrala cykel där spelaren agerar, spelet svarar och spelaren fortsätter. | Kapitel 14 |
| Git-tag | Markering av en specifik version i Git-historiken. | Kapitel 14 |

| Pre-build-checklista | Kontrollista som används innan en build skapas och delas. | Kapitel 14 |
| README | Kort textfil med instruktioner till den som ska testa prototypen. | Kapitel 14 |
| Git-tagg | Markering av en specifik version i Git-historiken, till exempel `v0.1`. | Kapitel 14 |


## Termer tillagda vid komplettering

| Term | Definition | Första kapitel |
|---|---|---|
| Coyote time | En kort tidsmarginal efter att spelaren lämnat marken där hopp fortfarande tillåts. | Kapitel 5 |
| Jump buffering | En kort minnesbuffert för hoppinput som gör att ett hopp kan utföras precis när spelaren landar. | Kapitel 5 |
| Merge conflict | En situation där Git inte automatiskt kan avgöra hur två ändringar ska kombineras. | Kapitel 13 |
