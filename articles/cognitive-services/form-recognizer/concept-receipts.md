---
title: Inleveranser-formulär igenkänning
titleSuffix: Azure Cognitive Services
description: Lär dig begrepp som rör inbetalnings analys med formulär tolkens API-användning och begränsningar.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: forms-recognizer
ms.topic: conceptual
ms.date: 08/17/2019
ms.author: pafarley
ms.openlocfilehash: 00709ca5e842e51edbf5b26c53fe0a18e80bb896
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 09/25/2020
ms.locfileid: "91262356"
---
# <a name="receipt-concepts"></a>Koncept för kvitton

Azure formulär tolken kan analysera kvitton med hjälp av en av sina förinställda modeller. Kvitto-API: et extraherar viktig information från försäljnings kvitton på engelska, till exempel handels namn, transaktions datum, transaktions summa, rad artiklar med mera. 

## <a name="understanding-receipts"></a>Förstå kvitton 

Många företag och individer är fortfarande beroende av manuell extrahering av data från deras försäljnings kvitton, oavsett om det gäller utgifts rapporter, åter betalningar, granskningar, skatte syfte, budgetering, marknadsföring eller annat. I dessa scenarier krävs ofta avbildningar av det fysiska inleveransen i validerings syfte.  

Det kan vara komplicerat att automatiskt extrahera data från dessa kvitton. Kvitton kan vara crumpleda och svåra att läsa, tryckta eller handskrivna delar och smartphone-bilder av kvitton kan vara låg kvalitet. Dessutom kan mallar och fält för kvitton variera avsevärt mellan marknad, region och handlare. Dessa utmaningar i både data extrahering och fält identifiering gör kvittot att bearbeta ett unikt problem.  

Med hjälp av optisk tecken läsning (OCR) och vår fördefinierade kvitto modell, aktiverar API för kvitto bearbetning och extraherar data från kvittona, t. ex. namn, tips, totalt, rad objekt med mera. Med det här API: et behöver du inte träna en modell som du bara skickar kvittot till API för att analysera kvitto och data extraheras.

![exempel kvitto](./media/contoso-receipt-small.png)

## <a name="what-does-the-receipt-api-do"></a>Vad gör kvitto-API: et? 

Det förskapade kvitto-API: et extraherar innehållet i försäljnings kvitton &mdash; som den typ av kvitto som du vanligt vis kommer till en restaurang, åter försäljare eller inköps lager.

### <a name="fields-extracted"></a>Extraherade fält

* Handels namn 
* Handels adress 
* Mobiltelefon nummer 
* Transaktions datum 
* Transaktions tid 
* Delsumma 
* Skatt 
* Totalt 
* Tips 
* Extrahering av rad objekt (till exempel artikelkvantitet, artikel pris, artikel namn)

### <a name="additional-features"></a>Ytterligare funktioner

Kvitto-API: t returnerar även följande information:

* Typ av kvitto (till exempel artikel, kredit kort och så vidare)
* Fält konfidensnivå (varje fält returnerar ett associerat konfidens värde)
* OCR RAW-text (OCR-extraherade textutdata för hela kvittot)
* Avgränsnings ruta för varje värde, rad och ord

## <a name="input-requirements"></a>Krav för indatamängd

[!INCLUDE [input reqs](./includes/input-requirements-receipts.md)]

## <a name="supported-locales"></a>Språk som stöds 

* **Förbyggd kvitto v 2.0** (ga) stöder försäljnings kvitton i en-US-språkvariant
* **Förskapad kvitto v 2.1 – för hands version. 1** (offentlig för hands version) lägger till ytterligare stöd för följande inleverans-språk: 
  * EN – AU 
  * EN-CA 
  * EN-GB 
  * EN-IN 

  > [!NOTE]
  > Språk information 
  >
  > Förbyggd kvitto v 2.1 – för hands version. 1 har en valfri begäran-parameter för att ange ett kvitto språk från ytterligare engelska marknader. För försäljnings kvitton på engelska från Australien (EN-AU), Kanada (EN-CA), Storbritannien (EN-GB) och Indien (EN-i) kan du ange språkvarianten för att få bättre resultat. Om ingen språkvariant anges i v 2.1 – för hands version. 1, kommer modellen att standardvärdet för EN-US-modell.


## <a name="the-analyze-receipt-operation"></a>Åtgärden analysera inleverans

I det här dokumentet [analyseras](https://westcentralus.dev.cognitive.microsoft.com/docs/services/form-recognizer-api-v2-1-preview-1/operations/AnalyzeReceiptAsync) en bild eller en PDF av ett kvitto som indata och det går att extrahera värdena för ränta och text. Anropet returnerar ett svars huvud fält som kallas `Operation-Location` . `Operation-Location`Värdet är en URL som innehåller det resultat-ID som ska användas i nästa steg.

|Svars huvud| Resultat-URL |
|:-----|:----|
|Åtgärds plats | `https://cognitiveservice/formrecognizer/v2.0/prebuilt/receipt/analyzeResults/56a36454-fc4d-4354-aa07-880cfbf0064f` |

## <a name="the-get-analyze-receipt-result-operation"></a>Resultat åtgärden för att analysera kvittot

Det andra steget är att anropa åtgärden för att [analysera mottagnings resultat](https://westcentralus.dev.cognitive.microsoft.com/docs/services/form-recognizer-api-v2-1-preview-1/operations/GetAnalyzeReceiptResult) . Den här åtgärden tar inmatat till det resultat-ID som skapades av åtgärden för att analysera kvittot. Den returnerar ett JSON-svar som innehåller ett **status** fält med följande möjliga värden. Du anropar den här åtgärden iterativt tills den returnerar värdet **lyckades** . Använd ett intervall på 3 till 5 sekunder för att undvika att överskrida antalet begär Anden per sekund (RPS).

|Fält| Typ | Möjliga värden |
|:-----|:----:|:----|
|status | sträng | notStarted: analys åtgärden har inte startats. |
| |  | körs: analys åtgärden pågår. |
| |  | misslyckades: det gick inte att utföra analysen. |
| |  | lyckades: analys åtgärden har slutförts. |

När fältet **status** har värdet **lyckades** inkluderar JSON-svaret mottagnings förståelse och text igenkännings resultat. Resultatet av mottagnings förståelsen är ordnat som en ord lista med namngivna fält värden där varje värde innehåller den extraherade texten, normaliserade värdet, avgränsnings rutan, relevansen och motsvarande Word-element. Resultatet av text igenkänningen är ordnat som en hierarki med rader och ord, med text, avgränsnings ram och information om säkerhet.

![exempel på mottagnings resultat](./media/contoso-receipt-2-information.png)

### <a name="sample-json-output"></a>Exempel på JSON-utdata

Se följande exempel på ett lyckat JSON-svar: noden "readResults" innehåller all den identifierade texten. Texten sorteras efter sida, sedan efter rad, sedan efter enskilda ord. Noden "documentResults" innehåller de affärskort-specifika värden som modellen identifierade. Här hittar du användbara nyckel/värde-par som förnamn, efter namn, företags namn med mera.

```json
{ 
  "status":"succeeded",
  "createdDateTime":"2019-12-17T04:11:24Z",
  "lastUpdatedDateTime":"2019-12-17T04:11:32Z",
  "analyzeResult":{ 
    "version":"2.0.0",
    "readResults":[ 
      { 
        "page":1,
        "angle":0.6893,
        "width":1688,
        "height":3000,
        "unit":"pixel",
        "language":"en",
        "lines":[ 
          { 
            "text":"Contoso",
            "boundingBox":[ 
              635,
              510,
              1086,
              461,
              1098,
              558,
              643,
              604
            ],
            "words":[ 
              { 
                "text":"Contoso",
                "boundingBox":[ 
                  639,
                  510,
                  1087,
                  461,
                  1098,
                  551,
                  646,
                  604
                ],
                "confidence":0.955
              }
            ]
          },
          ...
        ]
      }
    ],
    "documentResults":[ 
      { 
        "docType":"prebuilt:receipt",
        "pageRange":[ 
          1,
          1
        ],
        "fields":{ 
          "ReceiptType":{ 
            "type":"string",
            "valueString":"Itemized",
            "confidence":0.692
          },
          "MerchantName":{ 
            "type":"string",
            "valueString":"Contoso Contoso",
            "text":"Contoso Contoso",
            "boundingBox":[ 
              378.2,
              292.4,
              1117.7,
              468.3,
              1035.7,
              812.7,
              296.3,
              636.8
            ],
            "page":1,
            "confidence":0.613,
            "elements":[ 
              "#/readResults/0/lines/0/words/0",
              "#/readResults/0/lines/1/words/0"
            ]
          },
          "MerchantAddress":{ 
            "type":"string",
            "valueString":"123 Main Street Redmond, WA 98052",
            "text":"123 Main Street Redmond, WA 98052",
            "boundingBox":[ 
              302,
              675.8,
              848.1,
              793.7,
              809.9,
              970.4,
              263.9,
              852.5
            ],
            "page":1,
            "confidence":0.99,
            "elements":[ 
              "#/readResults/0/lines/2/words/0",
              "#/readResults/0/lines/2/words/1",
              "#/readResults/0/lines/2/words/2",
              "#/readResults/0/lines/3/words/0",
              "#/readResults/0/lines/3/words/1",
              "#/readResults/0/lines/3/words/2"
            ]
          },
          "MerchantPhoneNumber":{ 
            "type":"phoneNumber",
            "valuePhoneNumber":"+19876543210",
            "text":"987-654-3210",
            "boundingBox":[ 
              278,
              1004,
              656.3,
              1054.7,
              646.8,
              1125.3,
              268.5,
              1074.7
            ],
            "page":1,
            "confidence":0.99,
            "elements":[ 
              "#/readResults/0/lines/4/words/0"
            ]
          },
          "TransactionDate":{ 
            "type":"date",
            "valueDate":"2019-06-10",
            "text":"6/10/2019",
            "boundingBox":[ 
              265.1,
              1228.4,
              525,
              1247,
              518.9,
              1332.1,
              259,
              1313.5
            ],
            "page":1,
            "confidence":0.99,
            "elements":[ 
              "#/readResults/0/lines/5/words/0"
            ]
          },
          "TransactionTime":{ 
            "type":"time",
            "valueTime":"13:59:00",
            "text":"13:59",
            "boundingBox":[ 
              541,
              1248,
              677.3,
              1261.5,
              668.9,
              1346.5,
              532.6,
              1333
            ],
            "page":1,
            "confidence":0.977,
            "elements":[ 
              "#/readResults/0/lines/5/words/1"
            ]
          },
          "Items":{ 
            "type":"array",
            "valueArray":[ 
              { 
                "type":"object",
                "valueObject":{ 
                  "Quantity":{ 
                    "type":"number",
                    "text":"1",
                    "boundingBox":[ 
                      245.1,
                      1581.5,
                      300.9,
                      1585.1,
                      295,
                      1676,
                      239.2,
                      1672.4
                    ],
                    "page":1,
                    "confidence":0.92,
                    "elements":[ 
                      "#/readResults/0/lines/7/words/0"
                    ]
                  },
                  "Name":{ 
                    "type":"string",
                    "valueString":"Cappuccino",
                    "text":"Cappuccino",
                    "boundingBox":[ 
                      322,
                      1586,
                      654.2,
                      1601.1,
                      650,
                      1693,
                      317.8,
                      1678
                    ],
                    "page":1,
                    "confidence":0.923,
                    "elements":[ 
                      "#/readResults/0/lines/7/words/1"
                    ]
                  },
                  "TotalPrice":{ 
                    "type":"number",
                    "valueNumber":2.2,
                    "text":"$2.20",
                    "boundingBox":[ 
                      1107.7,
                      1584,
                      1263,
                      1574,
                      1268.3,
                      1656,
                      1113,
                      1666
                    ],
                    "page":1,
                    "confidence":0.918,
                    "elements":[ 
                      "#/readResults/0/lines/8/words/0"
                    ]
                  }
                }
              },
              ...
            ]
          },
          "Subtotal":{ 
            "type":"number",
            "valueNumber":11.7,
            "text":"11.70",
            "boundingBox":[ 
              1146,
              2221,
              1297.3,
              2223,
              1296,
              2319,
              1144.7,
              2317
            ],
            "page":1,
            "confidence":0.955,
            "elements":[ 
              "#/readResults/0/lines/13/words/1"
            ]
          },
          "Tax":{ 
            "type":"number",
            "valueNumber":1.17,
            "text":"1.17",
            "boundingBox":[ 
              1190,
              2359,
              1304,
              2359,
              1304,
              2456,
              1190,
              2456
            ],
            "page":1,
            "confidence":0.979,
            "elements":[ 
              "#/readResults/0/lines/15/words/1"
            ]
          },
          "Tip":{ 
            "type":"number",
            "valueNumber":1.63,
            "text":"1.63",
            "boundingBox":[ 
              1094,
              2479,
              1267.7,
              2485,
              1264,
              2591,
              1090.3,
              2585
            ],
            "page":1,
            "confidence":0.941,
            "elements":[ 
              "#/readResults/0/lines/17/words/1"
            ]
          },
          "Total":{ 
            "type":"number",
            "valueNumber":14.5,
            "text":"$14.50",
            "boundingBox":[ 
              1034.2,
              2617,
              1387.5,
              2638.2,
              1380,
              2763,
              1026.7,
              2741.8
            ],
            "page":1,
            "confidence":0.985,
            "elements":[ 
              "#/readResults/0/lines/19/words/0"
            ]
          }
        }
      }
    ]
  }
}
```


## <a name="customer-scenarios"></a>Kund scenarier  

De data som extraheras med kvitto-API kan användas för att utföra en rad olika uppgifter. Här följer några exempel på vad våra kunder har utfört med API: et för inleverans. 

### <a name="business-expense-reporting"></a>Rapportering av affärs utgifter  

Att arkivera företags kostnader innebär ofta att du anger data från bilder av kvitton manuellt. Med kvitto-API: et kan du använda de extraherade fälten för att delvis automatisera den här processen och snabbt analysera dina kvitton.  

Eftersom kvitto-API: et har ett enkelt JSON-utdata kan du använda de extraherade fältvärdena på flera sätt. Integrera med interna utgifts program för att fylla i utgifts rapporter i förväg. Mer information om det här scenariot får du genom att läsa om hur Acumatica använder sig av kvitto-API för att [göra utgifts rapportering till en mindre smärtsamt process](https://customers.microsoft.com/en-us/story/762684-acumatica-partner-professional-services-azure).  

### <a name="auditing-and-accounting"></a>Granskning och redovisning 

Utdata för API för kvittot kan också användas för att utföra analyser på ett stort antal utgifter vid olika tidpunkter i utgifts rapporteringen och åter betalnings processen. Du kan bearbeta kvitton för att prioritering dem för manuella granskningar eller snabba godkännanden.  

Kvitto utmatningen är också användbar för allmän bok för företag eller personligt bruk. Använd mottagnings-API: et för att transformera RAW-kvitto-bilder/PDF-data till digitala utdata som är åtgärds bara.

### <a name="consumer-behavior"></a>Konsument beteende 

Kvitton innehåller användbara data som du kan använda för att analysera konsumenternas beteende och shopping trender.

Kvitto-API: t ger också [AIBuilder för kvitto bearbetning](https://docs.microsoft.com/ai-builder/prebuilt-receipt-processing).

## <a name="next-steps"></a>Nästa steg

- Slutför snabb starten för [formulär tolkens klient bibliotek](quickstarts/client-library.md) för att komma igång med att skriva en app för mottagnings bearbetning med formulär tolken på valfritt språk.
- Eller så följer du in [kvitto-API python-snabb](./quickstarts/python-receipts.md) starten för att identifiera kvitton med hjälp av REST API.

## <a name="see-also"></a>Se även

* [Vad är formigenkänning?](./overview.md)
* [REST API referens dokument](https://westcentralus.dev.cognitive.microsoft.com/docs/services/form-recognizer/api)
