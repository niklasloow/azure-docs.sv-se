---
title: Visitkort – formulär igenkänning
titleSuffix: Azure Cognitive Services
description: Lär dig begrepp som är relaterade till visitkorts analys med formatet igenkännings-API-användning och begränsningar.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: forms-recognizer
ms.topic: conceptual
ms.date: 08/17/2019
ms.author: pafarley
ms.openlocfilehash: f8f173291448d9da4d8967ff56b0fa027ca73409
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 09/25/2020
ms.locfileid: "91334556"
---
# <a name="business-card-concepts"></a>Koncept för visitkort

Azure formulär tolken kan analysera och extrahera kontakt information från visitkorten med hjälp av en av dess förinställda modeller. Visitkorts-API: t kombinerar kraftfulla OCR-funktioner (optisk tecken läsning) med vårt affärskorts förståelse för modell för att extrahera viktig information från visitkort på engelska. Den extraherar personlig kontakt information, företags namn, befattning med mera. Det inbyggda API: t för visitkortet är offentligt tillgängligt i för hands versionen av formulär igenkänning v 2.1. 

## <a name="what-does-the-business-card-api-do"></a>Vad gör Business Card-API: et?

Visitkorts-API: n extraherar nyckel fält från visitkort och returnerar dem i ett organiserat JSON-svar.

![Contoso-specificerad bild från FOTT + JSON-utdata](./media/business-card-english.jpg)

### <a name="fields-extracted"></a>Extraherade fält:

* Kontakt namn 
  * Förnamn
  * Efter namn
* Företags namn 
* Avdelningar 
* Jobb titlar 
* E-postmeddelanden 
* Websites 
* Adresser 
* Telefonnummer 
  * Mobil telefoner 
  * Fax 
  * Telefon, arbete 
  * Andra telefoner 

Visitkorts-API: et kan också returnera all identifierad text från visitkortet. Denna OCR-utdata ingår i JSON-svaret.  

### <a name="input-requirements"></a>Krav för indatamängd 

[!INCLUDE [input reqs](./includes/input-requirements-receipts.md)]

## <a name="the-analyze-business-card-operation"></a>Åtgärden analysera visitkort

På [kortet analysera](https://westcentralus.dev.cognitive.microsoft.com/docs/services/form-recognizer-api-v2-1-preview-1/operations/AnalyzeBusinessCardAsync) får du en bild eller en PDF-fil med ett visitkort som indata och utvärderar intressena. Anropet returnerar ett svars huvud fält som kallas `Operation-Location` . `Operation-Location`Värdet är en URL som innehåller det resultat-ID som ska användas i nästa steg.

|Svars huvud| Resultat-URL |
|:-----|:----|
|Åtgärds plats | `https://cognitiveservice/formrecognizer/v2.1-preview.1/prebuilt/businessCard/analyzeResults/49a36324-fc4b-4387-aa06-090cfbf0064f` |

## <a name="the-get-analyze-business-card-result-operation"></a>Resultat åtgärden för att analysera affärs kort

Det andra steget är att anropa åtgärden för att [analysera visitkorts resultat](https://westcentralus.dev.cognitive.microsoft.com/docs/services/form-recognizer-api-v2-1-preview-1/operations/GetAnalyzeBusinessCardResult) . Den här åtgärden tar sig in i det resultat-ID som skapades av åtgärden analysera visitkort. Den returnerar ett JSON-svar som innehåller ett **status** fält med följande möjliga värden. Du anropar den här åtgärden iterativt tills den returnerar värdet **lyckades** . Använd ett intervall på 3 till 5 sekunder för att undvika att överskrida antalet begär Anden per sekund (RPS).

|Fält| Typ | Möjliga värden |
|:-----|:----:|:----|
|status | sträng | notStarted: analys åtgärden har inte startats.<br /><br />körs: analys åtgärden pågår.<br /><br />misslyckades: det gick inte att utföra analysen.<br /><br />lyckades: analys åtgärden har slutförts.|

När fältet **status** har värdet **lyckades** , inkluderar JSON-svaret affärs korts förståelse och valfria text igenkännings resultat om det behövs. Resultatet av visitkorten är ordnat som en ord lista med namngivna fält värden där varje värde innehåller den extraherade texten, normaliserade värdet, avgränsnings rutan, relevansen och motsvarande Word-element. Resultatet av text igenkänningen är ordnat som en hierarki med rader och ord, med text, avgränsnings ram och information om säkerhet.

![exempel på visitkorts resultat](./media/business-card-results.png)

### <a name="sample-json-output"></a>Exempel på JSON-utdata

Se följande exempel på ett lyckat JSON-svar: noden "readResults" innehåller all den identifierade texten. Texten sorteras efter sida, sedan efter rad, sedan efter enskilda ord. Noden "documentResults" innehåller de affärskort-specifika värden som modellen identifierade. Här hittar du användbar kontakt information, t. ex. förnamn, efter namn, företags namn med mera.

```json
{
    "status": "succeeded",
    "createdDateTime": "2020-08-20T17:41:19Z",
    "lastUpdatedDateTime": "2020-08-20T17:41:24Z",
    "analyzeResult": {
        "version": "2.1.0",
        "readResults": [
            {
                "page": 1,
                "angle": -17.0956,
                "width": 4032,
                "height": 3024,
                "unit": "pixel",
                   "lines": 
                             {
                        "text": "Dr. Avery Smith",
                        "boundingBox": [
                            419.3,
                            1154.6,
                            1589.6,
                            877.9,
                            1618.9,
                            1001.7,
                            448.6,
                            1278.4
                        ],
                        "words": [
                            {
                                "text": "Dr.",
                                "boundingBox": [
                                    419,
                            ]
    
            }
        ],
        "documentResults": [
            {
                "docType": "prebuilt:businesscard",
                "pageRange": [
                    1,
                    1
                ],
                "fields": {
                    "ContactNames": {
                        "type": "array",
                        "valueArray": [
                            {
                                "type": "object",
                                "valueObject": {
                                    "FirstName": {
                                        "type": "string",
                                        "valueString": "Avery",
                                        "text": "Avery",
                                        "boundingBox": [
                                            703,
                                            1096,
                                            1134,
                                            989,
                                            1165,
                                            1109,
                                            733,
                                            1206
                                        ],
                                        "page": 1
                                    },
                                    "LastName": {
                                        "type": "string",
                                        "valueString": "Smith",
                                        "text": "Smith",
                                        "boundingBox": [
                                            1186,
                                            976,
                                            1585,
                                            879,
                                            1618,
                                            998,
                                            1218,
                                            1096
                                        ],
                                        "page": 1
                                    }
                                },
                                "text": "Dr. Avery Smith",
                                "boundingBox": [
                                    419.3,
                                    1154.6,
                                    1589.6,
                                    877.9,
                                    1618.9,
                                    1001.7,
                                    448.6,
                                    1278.4
                                ],
                                "confidence": 0.97
                            }
                        ]
                    },
                    "JobTitles": {
                        "type": "array",
                        "valueArray": [
                            {
                                "type": "string",
                                "valueString": "Senior Researcher",
                                "text": "Senior Researcher",
                                "boundingBox": [
                                    451.8,
                                    1301.9,
                                    1313.5,
                                    1099.9,
                                    1333.8,
                                    1186.7,
                                    472.2,
                                    1388.7
                                ],
                                "page": 1,
                                "confidence": 0.99
                            }
                        ]
                    },
                    "Departments": {
                        "type": "array",
                        "valueArray": [
                            {
                                "type": "string",
                                "valueString": "Cloud & Al Department",
                                "text": "Cloud & Al Department",
                                "boundingBox": [
                                    480.1,
                                    1403.3,
                                    1590.5,
                                    1129.6,
                                    1612.6,
                                    1219.6,
                                    502.3,
                                    1493.3
                                ],
                                "page": 1,
                                "confidence": 0.99
                            }
                        ]
                    },
                    "Emails": {
                        "type": "array",
                        "valueArray": [
                            {
                                "type": "string",
                                "valueString": "avery.smith@contoso.com",
                                "text": "avery.smith@contoso.com",
                                "boundingBox": [
                                    2107,
                                    934,
                                    2917,
                                    696,
                                    2935,
                                    764,
                                    2126,
                                    995
                                ],
                                "page": 1,
                                "confidence": 0.99
                            }
                        ]
                    },
                    "Websites": {
                        "type": "array",
                        "valueArray": [
                            {
                                "type": "string",
                                "valueString": "https://www.contoso.com/",
                                "text": "https://www.contoso.com/",
                                "boundingBox": [
                                    2121,
                                    1002,
                                    2992,
                                    755,
                                    3014,
                                    826,
                                    2143,
                                    1077
                                ],
                                "page": 1,
                                "confidence": 0.995
                            }
                        ]
                    },
                    "MobilePhones": {
                        "type": "array",
                        "valueArray": [
                            {
                                "type": "phoneNumber",
                                "text": "+44 (0) 7911 123456",
                                "boundingBox": [
                                    2434.9,
                                    1033.3,
                                    3072,
                                    836,
                                    3096.2,
                                    914.3,
                                    2459.1,
                                    1111.6
                                ],
                                "page": 1,
                                "confidence": 0.99
                            }
                        ]
                    },
                    "OtherPhones": {
                        "type": "array",
                        "valueArray": [
                            {
                                "type": "phoneNumber",
                                "text": "+44 (0) 20 9876 5432",
                                "boundingBox": [
                                    2473.2,
                                    1115.4,
                                    3139.2,
                                    907.7,
                                    3163.2,
                                    984.7,
                                    2497.2,
                                    1192.4
                                ],
                                "page": 1,
                                "confidence": 0.99
                            }
                        ]
                    },
                    "Faxes": {
                        "type": "array",
                        "valueArray": [
                            {
                                "type": "phoneNumber",
                                "text": "+44 (0) 20 6789 2345",
                                "boundingBox": [
                                    2525,
                                    1185.4,
                                    3192.4,
                                    977.9,
                                    3217.9,
                                    1060,
                                    2550.5,
                                    1267.5
                                ],
                                "page": 1,
                                "confidence": 0.99
                            }
                        ]
                    },
                    "Addresses": {
                        "type": "array",
                        "valueArray": [
                            {
                                "type": "string",
                                "valueString": "2 Kingdom Street Paddington, London, W2 6BD",
                                "text": "2 Kingdom Street Paddington, London, W2 6BD",
                                "boundingBox": [
                                    1230,
                                    2138,
                                    2535.2,
                                    1678.6,
                                    2614.2,
                                    1903.1,
                                    1309,
                                    2362.5
                                ],
                                "page": 1,
                                "confidence": 0.977
                            }
                        ]
                    },
                    "CompanyNames": {
                        "type": "array",
                        "valueArray": [
                            {
                                "type": "string",
                                "valueString": "Contoso",
                                "text": "Contoso",
                                "boundingBox": [
                                    1152,
                                    1916,
                                    2293,
                                    1552,
                                    2358,
                                    1733,
                                    1219,
                                    2105
                                ],
                                "page": 1,
                                "confidence": 0.97
                            }
                        ]
                    }
                }
            }
        ]
    }
}
```

Följ instruktionerna för att [extrahera visitkorts data](./QuickStarts/python-business-cards.md) för att implementera data extrahering för visitkort med Python och REST API.

## <a name="customer-scenarios"></a>Kund scenarier  

De data som extraheras med visitkorts-API kan användas för att utföra en rad olika uppgifter. Om du extraherar den här kontakt informationen sparas automatiskt tid för dem i klient roller. Här följer några exempel på vad våra kunder har utfört med API: t för visitkort:

* Extrahera kontakt information från visitkort och skapa snabbt telefon kontakter. 
* Integrera med CRM för att automatiskt skapa kontakter med hjälp av visitkorts avbildningar. 
* Håll koll på Sälj leads.  
* Extrahera kontakt information i bulk från befintliga visitkorts avbildningar. 

Visitkorts-API: t ger också [AIBuilder för affärs korts bearbetning](https://docs.microsoft.com/ai-builder/prebuilt-business-card).

## <a name="next-steps"></a>Nästa steg

- Följ snabb starten för [Business Cards API python](./quickstarts/python-business-cards.md) för att komma igång med att känna igen visitkort.

## <a name="see-also"></a>Se även

* [Vad är formigenkänning?](./overview.md)
* [REST API referens dokument](https://westcentralus.dev.cognitive.microsoft.com/docs/services/form-recognizer-api-v2-1-preview-1/operations/AnalyzeBusinessCardAsync)
