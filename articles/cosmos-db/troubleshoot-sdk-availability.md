---
title: 'Diagnostisera och Felsök tillgängligheten för Azure Cosmos SDK: er i multiregionala miljöer'
description: Lär dig allt om tillgänglighets beteendet i Azure Cosmos SDK när du arbetar i flera regionala miljöer.
author: ealsur
ms.service: cosmos-db
ms.date: 09/24/2020
ms.author: maquaran
ms.subservice: cosmosdb-sql
ms.topic: troubleshooting
ms.reviewer: sngun
ms.openlocfilehash: 8dd7ced2dfcfd3c555555d6f0a197623bd8726f2
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 09/25/2020
ms.locfileid: "91330442"
---
# <a name="diagnose-and-troubleshoot-the-availability-of-azure-cosmos-sdks-in-multiregional-environments"></a>Diagnostisera och Felsök tillgängligheten för Azure Cosmos SDK: er i multiregionala miljöer

Den här artikeln beskriver beteendet för den senaste versionen av Azure Cosmos SDK: er när du ser ett anslutnings problem till en viss region eller när en regions växling inträffar.

Alla Azure Cosmos SDK: er ger dig möjlighet att anpassa de nationella inställningarna. Följande egenskaper används i olika SDK: er:

* Egenskapen [ConnectionPolicy. PreferredLocations](/dotnet/api/microsoft.azure.documents.client.connectionpolicy.preferredlocations) i .NET v2 SDK.
* Egenskaperna [CosmosClientOptions. ApplicationRegion](/dotnet/api/microsoft.azure.cosmos.cosmosclientoptions.applicationregion) eller [CosmosClientOptions. ApplicationPreferredRegions](/dotnet/api/microsoft.azure.cosmos.cosmosclientoptions.applicationpreferredregions) i .net v3 SDK.
* Metoden [CosmosClientBuilder. preferredRegions](/java/api/com.azure.cosmos.cosmosclientbuilder.preferredregions) i Java v4 SDK.
* Parametern [CosmosClient. preferred_locations](/python/api/azure-cosmos/azure.cosmos.cosmos_client.cosmosclient) i Node SDK.
* Parametern [CosmosClientOptions. ConnectionPolicy. preferredLocations](/javascript/api/@azure/cosmos/connectionpolicy#preferredlocations) i JS SDK.

När du anger den regionala inställningen ansluter klienten till en region som anges i följande tabell:

|Kontotyp |Läsningar |Skrivningar |
|------------------------|--|--|
| Enskild Skriv region | Önskad region | Primär region  |
| Flera Skriv regioner | Önskad region | Önskad region  |

Om du inte anger någon önskad region:

|Kontotyp |Läsningar |Skrivningar |
|------------------------|--|--|
| Enskild Skriv region | Primär region | Primär region |
| Flera Skriv regioner | Primär region  | Primär region  |

> [!NOTE]
> Primär region syftar på den första regionen i [Azure Cosmos-kontots region lista](distribute-data-globally.md)

När något av följande inträffar visar klienten som använder Azure Cosmos SDK loggar och innehåller information om återförsöket som en del av **åtgärdens diagnostiska information**:

* Egenskapen *RequestDiagnosticsString* i svar i .NET v2 SDK.
* Egenskapen *diagnostik* för svar och undantag i .net v3 SDK.
* Metoden *getDiagnostics ()* i svar och undantag i Java v4 SDK.

För en omfattande information om SLA-garantier under dessa händelser, se [service avtal för tillgänglighet](high-availability.md#slas-for-availability).

## <a name="removing-a-region-from-the-account"></a><a id="remove-region"></a>Ta bort en region från kontot

När du tar bort en region från ett Azure Cosmos-konto kommer alla SDK-klienter som använder kontot att identifiera regions borttagningen via en server dels svars kod. Klienten markerar sedan den regionala slut punkten som otillgänglig. Klienten försöker utföra den aktuella åtgärden igen och alla framtida åtgärder dirigeras permanent till nästa region i prioritetsordning.

## <a name="adding-a-region-to-an-account"></a>Lägga till en region i ett konto

Var 5: e minut läser Azure Cosmos SDK-klienten konto konfigurationen och uppdaterar de regioner som den är medveten om.

Om du tar bort en region och senare lägger till den i kontot igen, om den tillagda regionen har en högre regional prioritetsordning i SDK-konfigurationen än den aktuella anslutna regionen, kommer SDK att växla tillbaka till att använda den här regionen permanent. När regionen har identifierats dirigeras alla framtida begär anden till den.

Om du konfigurerar klienten för att helst ansluta till en region som Azure Cosmos-kontot inte har, ignoreras den önskade regionen. Om du lägger till den regionen senare identifierar klienten den och kommer att växla permanent till den regionen.

## <a name="failover-the-write-region-in-a-single-write-region-account"></a><a id="manual-failover-single-region"></a>Redundansväxla Skriv regionen i ett enda Skriv regions konto

Om du initierar en redundansväxling av den aktuella Skriv regionen kommer nästa Skriv förfrågan att Miss förfalla med ett känt Server dels svar. När det här svaret identifieras frågar klienten om kontot för att lära sig den nya Skriv regionen och fortsätter med att försöka utföra den aktuella åtgärden igen och dirigera alla framtida Skriv åtgärder till den nya regionen permanent.

## <a name="regional-outage"></a>Regionalt avbrott

Om kontot är en enda Skriv region och det regionala avbrottet inträffar under en Skriv åtgärd, liknar beteendet en [manuell redundansväxling](#manual-failover-single-region). För Läs begär Anden eller konton för flera Skriv regioner liknar beteendet att [ta bort en region](#remove-region).

## <a name="session-consistency-guarantees"></a>Konsekvens garanti för sessioner

När du använder konsekvens kontroll av [sessioner](consistency-levels.md#guarantees-associated-with-consistency-levels)måste klienten garantera att den kan läsa sina egna skrivningar. I ett konto med en Skriv region där inställningen Läs region skiljer sig från Skriv regionen, kan det finnas fall där användaren får en Skriv åtgärd och när en läsning görs från en lokal region, den lokala regionen har ännu inte tagit emot datareplikeringen (hastighet för ljus begränsning). I sådana fall identifierar SDK det speciella fel som finns i Läs åtgärden och gör om läsningen på Hub-regionen för att säkerställa konsekvensen i sessionen.

## <a name="transient-connectivity-issues-on-tcp-protocol"></a>Tillfälliga anslutnings problem i TCP-protokollet

I scenarier där Azure Cosmos SDK-klienten är konfigurerad för att använda TCP-protokollet för en viss begäran, kan det finnas situationer där nätverks förhållandena tillfälligt påverkar kommunikationen med en viss slut punkt. Dessa tillfälliga nätverks villkor kan vara en yta som TCP-tidsgräns. Klienten gör om begäran lokalt på samma slut punkt under några sekunder.

Om användaren har konfigurerat en lista över önskade regioner med mer än en region och Azure Cosmos-kontot är flera Skriv regioner eller en enskild Skriv region och åtgärden är en Read-begäran, försöker klienten igen med en enda åtgärd i nästa region från listan över inställningar.

## <a name="next-steps"></a>Nästa steg

* Granska [tillgänglighets service avtal](high-availability.md#slas-for-availability).
* Använd den senaste [.NET SDK: n](sql-api-sdk-dotnet-standard.md)
* Använd den senaste [Java SDK: n](sql-api-sdk-java-v4.md)
* Använd den senaste [python SDK: n](sql-api-sdk-python.md)
* Använd den senaste [NODEN SDK](sql-api-sdk-node.md)
