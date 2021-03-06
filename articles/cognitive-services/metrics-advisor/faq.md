---
title: Vanliga frågor och svar om mått rådgivare
titleSuffix: Azure Cognitive Services
description: Vanliga frågor och svar om Metrics Advisor-tjänsten.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: metrics-advisor
ms.topic: conceptual
ms.date: 09/10/2020
ms.author: aahi
ms.openlocfilehash: 0fde9a0f46073a2f3a24962ea58431581455f474
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 09/22/2020
ms.locfileid: "90941532"
---
# <a name="metrics-advisor-frequently-asked-questions"></a>Vanliga frågor och svar om mått rådgivare

### <a name="what-is-the-cost-of-my-instance"></a>Vad kostar min instans?

Det finns för närvarande ingen kostnad för att använda din instans under för hands versionen.

### <a name="why-is-the-demo-website-readonly"></a>Varför är demonstrations webbplatsen skrivskyddad?

[Demo webbplatsen](https://anomaly-detector.azurewebsites.net/) är offentligt tillgänglig. Den här instansen blir skrivskyddad för att förhindra oavsiktlig överföring av data.

### <a name="why-cant-i-create-the-resource-the-pricing-tier-is-unavailable-and-it-says-you-have-already-created-1-s0-for-this-subscription"></a>Varför kan jag inte skapa resursen? Pris nivån är inte tillgänglig och säger att du redan har skapat 1 S0 för den här prenumerationen?

:::image type="content" source="media/pricing.png" alt-text="Meddelande när det redan finns en F0-resurs":::

Under en offentlig för hands version får endast en instans av Metrics Advisor skapas under en prenumeration i en region.

Om du redan har en instans som skapats i samma region med samma prenumeration kan du prova med en annan region eller en annan prenumeration för att skapa en ny instans. Du kan också ta bort en befintlig instans om du vill skapa en ny.

Om du redan har tagit bort den befintliga instansen men ändå ser felet, vänta i ungefär 20 minuter efter att resursen tagits bort innan du skapar en ny instans.

## <a name="basic-concepts"></a>Grundläggande begrepp

### <a name="what-is-multi-dimensional-time-series-data"></a>Vad är flerdimensionella Time Series-data?

Se definitionen av [flerdimensionell mått](glossary.md#multi-dimensional-metric)  i ord listan.

### <a name="how-much-data-is-needed-for-metrics-advisor-to-start-anomaly-detection"></a>Hur mycket data behövs för att Metric Advisor ska starta avvikelse identifiering?

En data punkt kan som minimum utlösa avvikelse identifiering. Detta ger dock inte bästa möjliga noggrannhet. Tjänsten kommer att anta ett fönster med tidigare data punkter med det värde som du har angett som "fyllnings Lucke"-regel när datafeed skapas.

Vi rekommenderar att du har några data före den tidstämpel som du vill identifiera.
Baserat på data precisionen varierar den rekommenderade data mängden enligt nedan.

| Precision | Rekommenderat data belopp för identifiering |
| ----------- | ------------------------------------- |
| Mindre än 5 minuter | 4 dagars data |
| 5 minuter till 1 dag | 28 dagars data |
| Mer än 1 dag, till 31 dagar | 4 års data |
| Mer än 31 dagar | 48 års data |

### <a name="why-metrics-advisor-doesnt-detect-anomalies-from-historical-data"></a>Varför är metric Advisor inte identifiera avvikelser från historiska data?

Metrics Advisor är utformad för att identifiera direkt strömmande data. Det finns en begränsning av den maximala längden för historiska data som tjänsten kommer att se tillbaka och köra avvikelse identifiering. Det innebär att endast data punkter efter en viss tidigaste tidstämpel har avvikelse identifierings resultat. Den tidigaste tidsstämpeln beror på datans granularitet.

Baserat på data granularitet är längden på historiska data som har avvikelse identifierings resultat som nedan.

| Precision | Maximal längd på historiska data för avvikelse identifiering |
| ----------- | ------------------------------------- |
| Mindre än 5 minuter | Drift tid – 13 timmar |
| 5 minuter till mindre än 1 timme | Onboard Time-4 dagar  |
| 1 timme till mindre än 1 dag | Drift tid – 14 dagar  |
| 1 dag | Onboard Time – 28 dagar  |
| Mer än 1 dag, mindre än 31 dagar | Onboard Time – 2 år  |
| Mer än 31 dagar | Drift tid – 24 år   |

### <a name="more-concepts-and-technical-terms"></a>Fler begrepp och tekniska villkor

Gå till [ord listan](glossary.md) om du vill läsa mer.

## <a name="how-do-i-detect-such-kinds-of-anomalies"></a>Hur gör jag för att identifiera sådana typer av avvikelser? 

### <a name="how-do-i-detect-spikes--dips-as-anomalies"></a>Hur gör jag för att identifiera toppar & DIP som avvikelser?

Om du har hård tröskelvärden fördefinierade kan du faktiskt ange "hård tröskel" i konfigurationer för [avvikelse identifiering](how-tos/configure-metrics.md#anomaly-detection-methods).
Om det inte finns några tröskelvärden kan du använda "Smart identifiering" som drivs av AI. Mer information finns i [finjustera konfigurationen för identifiering](how-tos/configure-metrics.md#tune-the-detecting-configuration) .

### <a name="how-do-i-detect-inconformity-with-regular-seasonal-patterns-as-anomalies"></a>Hur gör jag för att identifiera inkonsekvenser med regelbundna (säsongs) mönster som avvikelser?

"Smart identifiering" kan lära sig mönstret för dina data, inklusive säsongs mönster. Den identifierar sedan de data punkter som inte uppfyller vanliga mönster som avvikelser. Mer information finns i [finjustera konfigurationen för identifiering](how-tos/configure-metrics.md#tune-the-detecting-configuration) .

### <a name="how-do-i-detect-flat-lines-as-anomalies"></a>Hur gör jag för att identifiera enkla linjer som avvikelser?

Om dina data normalt är instabila och varierar mycket, och du vill bli aviserad om det blir för stabilt eller till och med en plan, kan "ändra tröskel" konfigureras för att identifiera sådana data punkter när ändringen är för liten.
Mer information finns i [konfigurationer för avvikelse identifiering](how-tos/configure-metrics.md#anomaly-detection-methods) .

## <a name="next-steps"></a>Efterföljande moment
- [Översikt över Metrics Advisor](overview.md)
- [Prova demo webbplatsen](quickstarts/explore-demo.md)
- [Använda webbportalen](quickstarts/web-portal.md)