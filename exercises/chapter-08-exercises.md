# Övningar till kapitel 8: Fiender och hinder

## Övning 1: Patrullhastighet

1. Välj en fiende i scenen.
2. Testa tre olika värden på `moveSpeed`.
3. Spela banan efter varje ändring.
4. Skriv ner vilket värde som känns bäst.

Reflektera:
- När blev fienden för enkel?
- När blev fienden orättvis?
- Vilket värde gav bäst spelkänsla?

## Övning 2: Farliga objekt

Skapa två olika hinder som använder `DamageZone`.

Exempel:
- taggar på marken
- farlig buske
- fallzon under banan

Krav:
- objektet ska ha `Collider2D`
- `Is Trigger` ska vara aktiverat
- `DamageZone` ska vara kopplat
- Console ska visa meddelande när spelaren rör vid objektet

## Övning 3: Patrullsträcka

Flytta patrullpunkterna så att fienden rör sig över en längre sträcka.

Testa sedan banan och svara:
1. Finns det en säker plats där spelaren kan vänta?
2. Går det att förstå fiendens mönster?
3. Behöver plattformen vara bredare?

## Fördjupning: Vänd fienden

Försök få fiendens sprite att vända sig när fienden byter riktning.

Ledtråd:
- jämför den nya målpunkten med fiendens position
- ändra `localScale.x` beroende på riktning
- testa noga så att fienden inte råkar ändra storlek varje gång den vänder
