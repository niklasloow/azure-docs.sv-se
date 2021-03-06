---
title: Funktioner i Azure Monitor logg frågor | Microsoft Docs
description: Den här artikeln beskriver hur du använder funktioner för att anropa en fråga från en annan logg fråga i Azure Monitor.
ms.subservice: logs
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 07/31/2020
ms.openlocfilehash: 155f27b9f04db74955640aad8f582fdf05539368
ms.sourcegitcommit: 29400316f0c221a43aff3962d591629f0757e780
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/02/2020
ms.locfileid: "87511946"
---
# <a name="using-functions-in-azure-monitor-log-queries"></a>Använda funktioner i Azure Monitor logg frågor

Om du vill använda en logg fråga med en annan fråga kan du spara den som en funktion. På så sätt kan du förenkla komplexa frågor genom att dela upp dem i delar och du kan återanvända gemensam kod med flera frågor.

## <a name="create-a-function"></a>Skapa en funktion

Skapa en funktion med Log Analytics i Azure Portal genom att klicka på **Spara** och sedan ange informationen i följande tabell.

| Inställningen | Beskrivning |
|:---|:---|
| Name           | Visnings namn för frågan i **query Explorer**. |
| Spara som        | Funktion |
| Funktions Ali Aset | Kort namn för att använda funktionen i andra frågor. Får inte innehålla blank steg och måste vara unikt. |
| Kategori       | En kategori för att organisera sparade frågor och funktioner i **query Explorer**. |




## <a name="use-a-function"></a>Använd en funktion
Använd en funktion genom att inkludera dess alias i en annan fråga. Den kan användas som vilken annan tabell som helst.

## <a name="function-parameters"></a>Funktions parametrar 
Du kan lägga till parametrar till en funktion så att du kan ange värden för vissa variabler när du anropar den. Det enda sättet för att skapa en funktion med parametrar är att använda en Resource Manager-mall. Se exempel på [Resource Manager-mallar för logg frågor i Azure Monitor](../samples/resource-manager-log-queries.md#parameterized-function) .

## <a name="example"></a>Exempel
Följande exempel fråga returnerar alla saknade säkerhets uppdateringar som rapporter ATS under den senaste dagen. Spara den här frågan som en funktion med aliaset _security_updates_last_day_. 

```Kusto
Update
| where TimeGenerated > ago(1d) 
| where Classification == "Security Updates" 
| where UpdateState == "Needed"
```

Skapa en ny fråga och referera till funktionen _security_updates_last_day_ för att söka efter SQL-relaterade nödvändiga säkerhets uppdateringar.

```Kusto
security_updates_last_day | where Title contains "SQL"
```

## <a name="next-steps"></a>Nästa steg
Se andra lektioner för att skriva Azure Monitor logg frågor:

- [Strängåtgärder](string-operations.md)
- [Åtgärder för datum och tid](datetime-operations.md)
- [Aggregeringsfunktioner](aggregations.md)
- [Avancerade aggregeringar](advanced-aggregations.md)
- [JSON och datastrukturer](json-data-structures.md)
- [Kopplingar](joins.md)
- [Diagram](charts.md)
