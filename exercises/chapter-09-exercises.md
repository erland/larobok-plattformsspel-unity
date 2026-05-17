# Övningar till kapitel 9: Hälsa, liv och checkpoints

## Övning 1: Skapa ett fungerande hälsosystem

1. Lägg till `PlayerHealth` på spelaren.
2. Sätt `Max Health` till 3.
3. Uppdatera minst ett farligt objekt så att det använder `DamageZone`.
4. Testa att hälsan minskar i Console.
5. Kontrollera att spelaren återuppstår när hälsan når noll.

## Övning 2: Bygg en checkpointsekvens

Skapa en kort bana med:

- startposition
- enkel plattform
- checkpoint
- fiende eller hinder
- fallzon under banan

Testa att spelaren återvänder till checkpointen efter att checkpointen aktiverats.

## Övning 3: Variera skadan

Ge olika faror olika värden för `Damage Amount`.

Förslag:

| Objekt | Damage Amount |
|---|---|
| Enkel fiende | 1 |
| Taggar | 2 |
| Fallzon | 3 |

Spela igenom banan och fundera på vilka värden som känns rättvisa.

## Reflektionsfrågor

1. Vilket ansvar har `PlayerHealth`?
2. Vilket ansvar har `DamageZone`?
3. Varför är det bra att checkpoints inte själva flyttar spelaren?
4. Hur skulle du visa spelarens hälsa utan att använda Console?
5. Var bör checkpoints placeras för att banan ska kännas rättvis?

## Frivillig utmaning

Skapa en checkpoint som bara kan aktiveras en gång och skriver ett annat meddelande i Console om spelaren går igenom den igen.
