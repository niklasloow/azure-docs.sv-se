---
title: Begär ande gränser – översättare
titleSuffix: Azure Cognitive Services
description: I den här artikeln visas begränsningar för begäran för Translator. Avgifter debiteras baserat på antal tecken, inte begär ande frekvens med en gräns på 5 000 tecken per begäran. Tecken begränsningar är prenumerationer baserat, med F0 begränsad till 2 000 000 tecken per timme.
services: cognitive-services
author: swmachan
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: conceptual
ms.date: 08/06/2020
ms.author: swmachan
ms.openlocfilehash: dff69f00833cc3f8004befd330d144f4da0e332d
ms.sourcegitcommit: d0541eccc35549db6381fa762cd17bc8e72b3423
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 09/09/2020
ms.locfileid: "89566593"
---
# <a name="request-limits-for-translator"></a>Begär ande begränsningar för översättare

Den här artikeln innehåller begränsningar för begränsning av Translator. Tjänsterna omfattar översättning, transkriberingsspråk, identifiering av menings längd, språk identifiering och alternativa översättningar.

## <a name="character-and-array-limits-per-request"></a>Begränsningar för tecknen och matrisen per begäran

Varje översättnings förfrågan är begränsad till 10 000 tecken, på alla mål språk som du översätter till. Om du till exempel skickar en översättnings förfrågan på 3 000 tecken som ska översättas till tre olika språk resulterar det i en begär ande storlek på 3000x3 = 9 000 tecken som uppfyller gränsen för begäran. Du debiteras per steg, inte av antalet begär Anden. Vi rekommenderar att du skickar kortare begär Anden.

I följande tabell visas mat ris element och Character-begränsningar för varje operation av Translator.

| Åtgärd | Maximal storlek för mat ris element |    Maximalt antal mat ris element |    Maximal begär ande storlek (tecken) |
|:----|:----|:----|:----|
| Översätt | 10 000    | 100   | 10 000 |
| Transkribera | 5 000 | 10    | 5 000 |
| Upptäcka | 50 000 | 100 |   50 000 |
| BreakSentence | 50 000    | 100 | 50 000 |
| Slå upp i ordlista| 100 |  10  | 1 000 |
| Ordlisteexempel | 100 för text och 100 för översättning (totalt 200)| 10|   2 000 |

## <a name="character-limits-per-hour"></a>Character-gränser per timme

Din tecken gräns per timme baseras på prenumerations nivå för översättare. 

Tim kvoten bör förbrukas jämnt under hela timmen. Till exempel, på F0-nivå gränsen på 2 000 000 tecken per timme, ska tecken förbrukas inte snabbare än med ungefär 33 300 tecken per minut i glidande fönster (2 000 000-tecken dividerat med 60 minuter).

Om du når eller överskrider de här gränserna eller skickar för stor del av kvoten under en kort tids period får du troligt vis ett kvot svar. Det finns inga begränsningar för samtidiga begär Anden.

| Nivå | Tecken gräns |
|------|-----------------|
| F0 | 2 000 000 tecken per timme |
| S1 | 40 000 000 tecken per timme |
| S2/C2 | 40 000 000 tecken per timme |
| S3/C3 | 120 000 000 tecken per timme |
| S4/C4 | 200 000 000 tecken per timme |

Begränsningar för [prenumerationer med flera tjänster](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-reference#authentication) är samma som i S1-nivån.

Dessa gränser är begränsade till Microsofts standard översättnings modeller. Anpassade översättnings modeller som använder anpassad översättare är begränsade till 1 800 tecken per sekund.

## <a name="latency"></a>Svarstid

Translator har en maximal fördröjning på 15 sekunder med standard modeller och 120 sekunder när du använder anpassade modeller. Vanligt vis returneras svar *för text på 100 tecken* i 150 millisekunder till 300 millisekunder. De anpassade översättnings modellerna har liknande svars tids krav för den varaktiga begär ande frekvensen och kan ha en högre latens när din begär ande frekvens är intermittent. Svars tiderna varierar beroende på storleken på begäran och språk paret. Om du inte får någon översättning eller ett [felsvar](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-reference#errors) inom den tids ramen, kontrol lera din kod, din nätverks anslutning och försök igen. 

## <a name="sentence-length-limits"></a>Längd begränsningar för mening

När du använder funktionen [BreakSentence](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-break-sentence) är menings längden begränsad till 275 tecken. Det finns undantag för följande språk:

| Språk | Kod | Tecken gräns |
|----------|------|-----------------|
| Kinesiska | zh | 166 |
| Tyska | de | 800 |
| Italienska | it | 800 |
| Japanska | ja | 166 |
| Portugisiska | pt | 800 |
| Spanska | es | 800 |
| Thailändska | i:te | 180 |

> [!NOTE]
> Den här begränsningen gäller inte för översättningar.

## <a name="next-steps"></a>Nästa steg

* [Prissättning](https://azure.microsoft.com/pricing/details/cognitive-services/translator-text-api/)
* [Regional tillgänglighet](https://azure.microsoft.com/global-infrastructure/services/?products=cognitive-services)
* [Referens för Translator v3](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-reference)
