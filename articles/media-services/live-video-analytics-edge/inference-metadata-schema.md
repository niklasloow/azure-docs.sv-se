---
title: Schema för härledning av metadata – Azure
description: I den här artikeln får du lära dig mer om schema för data härlednings metadata.
ms.topic: overview
ms.date: 09/14/2020
ms.openlocfilehash: 6239713fc92b75b8ed026a8f04953e92a24c4596
ms.sourcegitcommit: 56cbd6d97cb52e61ceb6d3894abe1977713354d9
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/20/2020
ms.locfileid: "88691951"
---
# <a name="inference-metadata-schema"></a>Schema för härledning av metadata 

Varje Härlednings objekt oavsett om du använder HTTP-baserat kontrakt eller gRPC-baserat kontrakt följer objekt modellen som beskrivs i det här avsnittet.

## <a name="object-model"></a>Objekt modell

![Objekt modell](./media/inference-metadata-schema/object-model.png)
 
|Typdefinition|Beskrivning|
|---|---|
|Tagga|Tagg eller etikett som är associerad med resultatet. Alng med taggning kan du även hämta det konfidens värde som är associerat med taggen.|
|Attribut|Ytterligare attribut som är associerade med resultatet. Du kan lägga till nya attribut som du får från inferencing-motorn tillsammans med konfidens värdet.|
|Attributlistan|Lista över valfria attribut.|
|Rektangel|Rektangulär region i förhållande till bildens övre vänstra hörn. De obligatoriska egenskaperna är "Length", "width", height "och" övre kant avstånd från ursprunget ".|
|Klassificering|Etiketten på klassificeraren används ofta för hela exemplet. Med hjälp av "tag" kan du klassificera resultatet.|
|Entitet|Entitet identifierad eller identifierad i exemplet. När en entitet identifieras av inferencing-motorn, hämtas en "tag", ytterligare attribut som har härletts och co-koordinaterna i en rektangulär ruta runt den hittade entiteten som hittas returneras.|
|Händelse|Händelse upptäcktes i exemplet. När en händelse identifieras i exemplet returneras namnet på händelsen och de speciella egenskaperna för händelsen.|
|Sig|Rörelse upptäcktes i exemplet. När motion identifieras i exemplet returneras samkoordinaterna för en rektangulär avgränsnings ruta där rörelse har identifierats.|
|Text|Text som är associerad med exemplet tillsammans med textens start-och slut-tidstämpel returneras.|
|Annat|Returnerar annan allmän nytto Last information.|

Exemplet nedan innehåller en enskild händelse med vissa typer av härlednings typer som stöds:

```
[ 
  // Light Detection 
  { 
    "type": "classification", 
    "subtype": "lightDetection", 
    "classification": { 
      "tag": { "value": "daylight", "confidence": 0.86 }, 
      "attributes": [ 
          { "name": "isBlackAndWhite", "value": "false", "confidence": 0.71 } 
      ] 
    } 
  }, 
 
  // Motion Detection 
  { 
    "type": "motion", 
    "subtype": "motionDetection", 
    "motion": 
    { 
      "box": { "l": 0.0, "t": 0.0, "w": 0.0, "h": 0.0 } 
    } 
  }, 
 
  // Yolo V3 
  { 
    "type": "entity", 
    "subtype": "objectDetection",     
    "entity": 
    { 
      "tag": { "value": "dog", "confidence": 0.97 }, 
      "box": { "l": 0.0, "t": 0.0, "w": 0.0, "h": 0.0 } 
    } 
  }, 
 
  // Vehicle Identification 
  { 
    "type": "entity", 
    "subtype": "vehicleIdentification",     
    "entity": 
    { 
      "tag": { "value": "007-SPY", "confidence": 0.82 }, 
      "attributes":[   
        { "name": "color", "value": "black", "confidence": 0.90 }, 
        { "name": "body", "value": "coupe", "confidence": 0.87 }, 
        { "name": "make", "value": "Aston Martin", "confidence": 0.35 }, 
        { "name": "model", "value": "DBS V12", "confidence": 0.33 } 
      ], 
      "box": { "l": 0.0, "t": 0.0, "w": 0.0, "h": 0.0 } 
    } 
  }, 
 
  // People Identification 
  { 
    "type": "entity", 
    "subtype": "peopleIdentification",     
    "entity": 
    { 
      "tag": { "value":"Erwin Schrödinger", "confidence": 0.50 }, 
      "attributes":[   
        { "name": "age", "value": "73", "confidence": 0.87 }, 
        { "name": "glasses", "value": "yes", "confidence": 0.94 } 
      ], 
      "box": { "l": 0.0, "t": 0.0, "w": 0.0, "h": 0.0 } 
    }, 
 
    // Open type coming from the gRPC Map 
    "extensions":  
    { 
      "vector": "e1xkaXNwbGF5c3R5bGUgaVxoYmFyIHtcZnJhYyB7ZH17ZHR9fVx2ZXJ0IFxQc2kgKHQpXHJhbmdsZSA9e1xoYXQge0h9fVx2ZXJ0IFxQc2kgKHQpXHJhbmdsZSB9KQ==", 
      "skeleton": "p1,p2,p3,p4" 
    } 
  }, 
 
  // Captions 
  {     
    "type": "text", 
    "subtype": "speechToText",   
    "text": 
    { 
      "value": "Humor 75%. Confirmed. Self-destruct sequence in T minus 10… 9…", 
      "language": "en-US", 
      "startTimestamp": 12000, 
      "endTimestamp": 13000 
    } 
]
```

## <a name="next-steps"></a>Nästa steg

- [gRPC data kontrakt](grpc-data-contract.md)
- [HTTP-datakontrakt](http-data-contract.md)
