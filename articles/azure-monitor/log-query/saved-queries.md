---
title: Sparade frågor i Azure Monitor Log Analytics
description: Frågor som du kan starta från och ändra efter behov
ms.subservice: logs
ms.topic: article
author: rboucher
ms.author: robb
ms.date: 06/16/2020
ms.openlocfilehash: 462358ab967d4ba26a265d7b5f401b995223815e
ms.sourcegitcommit: 845a55e6c391c79d2c1585ac1625ea7dc953ea89
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/05/2020
ms.locfileid: "85959850"
---
# <a name="saved-queries-in-azure-monitor-log-analytics"></a>Sparade frågor i Azure Monitor Log Analytics

Log Analytics erbjuder uppsättningar med exempel frågor som du kan köra på egen hand eller använda som utgångs punkt för dina egna frågor. I den här artikeln beskrivs exempel frågor och hur du använder dem.

Om du inte är bekant med Log Analytics eller KQL-frågespråket, är exempel frågorna ett bra sätt att starta. De kan ge direkt insikt i en resurs och tillhandahålla ett bra sätt att börja lära sig och använda KQL, vilket gör att den tid det tar att börja använda Log Analytics. Vi har samlat in och granskat över 250 exempel frågor som har utformats för att ge dig ett omedelbart värde och det antal exempel frågor som ökar ständigt.

## <a name="in-context-queries"></a>Kontext frågor

Den nya upplevelsen filtrerar och föreslår frågor i kontexten. Med andra ord visar systemet automatiskt endast frågor som är relevanta för den omfattning som du har valt.

- För en **enskild resurs** – frågor filtreras enligt resurs typen.
- För en **resurs grupp** – frågor filtreras enligt resurserna i den angivna resurs gruppen.
- För en **arbets yta** – frågor filtreras enligt de lösningar som är installerade på arbets ytan.

Detta beteende är konsekvent för alla Log Analytics omfång. Om du inte ser en exempel fråga för den resurs typ som du vill använda kan det bero på att filter är i kontexten. Ett senare avsnitt beskriver hur du tar bort kontext omfånget så att du kan visa alla möjliga frågor.

> [!TIP]
> De mer resurser som du har i ditt omfång är längre tid för portalen att filtrera och Visa dialog rutan exempel fråga.

## <a name="example-query-user-interface"></a>Exempel fråga efter användar gränssnitt

Du kan komma till exempel frågor från två olika platser.

### <a name="example-query-dialog"></a>Exempel fråga-dialog ruta

När du först anger den Log Analytics upplevelsen visas *dialog rutan exempel frågor* automatiskt.  Du kan också komma åt det genom att klicka längst upp till höger på skärmen i **exempel frågor**.

![Marginal](media/saved-queries/sidebar-2.png)

Exempel fråga-dialog rutan visas som visas nedan:  

![Exempel på frågor-skärmen](media/saved-queries/example-query-start.png)

På föregående skärm bild visas skärmen loggar för en Azure Key Vault-instans. Som nämnts tidigare i den här artikeln visas frågorna i kontext.  Därför visar skärm bilden bara Key Vault relaterade exempel. Om du väljer en hel prenumeration visas frågor för alla resurs typer i den prenumerationen.  

Varje exempel fråga representeras av ett kort. Du kan snabbt söka igenom frågorna och hitta vad du behöver. Du kan köra frågan direkt från dialog rutan eller välja att läsa in den till Frågeredigeraren för ytterligare fin justering och anpassning.

### <a name="sidebar-query-experience"></a>Fråge funktionen i sido rutan

Alla funktioner i dialog rutan kan nås från fönstret frågor i den vänstra marginal listen för Log Analytics. Du kan hovra över ett frågenamn för att hämta en beskrivning av frågan och ytterligare funktioner.

![Marginal](media/saved-queries/sidebar-3.png)

## <a name="finding-and-filtering-queries"></a>Söka efter och filtrera frågor

Alternativen i det här avsnittet är tillgängliga både i dialog rutan och i sido Rute frågan, men med ett något annorlunda användar gränssnitt.  

### <a name="use-favorites"></a>Använd favoriter

Du kan använda vanliga frågor för att ge dig snabbare åtkomst.

> [!TIP]
> Att samla in och Visa frågor senare är ett bra sätt att komma igång. Hitta de frågor du behöver och klicka på stjärnan bredvid frågan för att lägga till den i favoriter. Om du senare upptäcker att frågan inte är användbar för dig, kan du avpublicera den.  

### <a name="filtering-and-group-by"></a>Filtrera och gruppera efter

Även om filerna i dialog rutan för fråg ETA data endast visar frågor med rätt kontext, kan du välja att ta bort filtret och se alla frågor.

### <a name="group-by"></a>Gruppera efter

Ändra grupperingen av frågorna genom att klicka på list rutan *Gruppera efter* :

![Exempel frågor skärm groupby](media/saved-queries/example-query-groupby.png)

Dialog rutan stöder gruppering av:

- **Resurs typ** – en resurs som definieras i Azure, till exempel en virtuell dator. Se [referensen Azure Monitor tabell](/azure/azure-monitor/reference/tables/tables-resourcetype) för en fullständig mappning av Azure Monitors loggar/Log Analytics tabeller till resurs typ.  
- **Kategori** – en typ av information som *säkerhet* eller *granskning*. Kategorier är identiska med de kategorier som definierats i tabellens sida fönster. Se [referensen Azure Monitor tabell](/azure/azure-monitor/reference/tables/tables-category) för en fullständig lista över kategorier.  
- **Lösning** – en Azure Monitor lösning som är associerad med frågorna
- **Ämne** – ämnet i exempel frågan, till exempel *aktivitets loggar* eller *app-loggar*. Egenskapen ämne är unik för exempel frågor och kan skilja sig åt beroende på den specifika resurs typen.

De grupperade värdena fungerar också som en aktiv innehålls förteckning. Om du klickar på ett av värdena till vänster på skärmen, rullas vyn frågor till objektet som du klickar på.

### <a name="filter"></a>Filter

Du kan också filtrera frågorna enligt de groupby-värden som anges ovan. I dialog rutan exempel fråga hittar du filtren högst upp.

![Exempel frågor skärm filter](media/saved-queries/example-query-filter.png)

### <a name="combining-group-by-and-filter"></a>Kombinera Group by och filter

Funktionen Filtrera och gruppera efter är utformad för att fungera i tandem. De ger flexibilitet i hur frågor ordnas. Om du till exempel använder en resurs grupp med flera resurser, kanske du vill filtrera ned till en speciell resurs typ och ordna de resulterande frågorna efter ämne.

## <a name="sample-query-dialog-appearance-behavior"></a>Exempel på dialog rutan för frågans utseende

Om du är KQL Pro och föredrar att komma direkt till Frågeredigeraren kan du växla dialog rutan för frågor. Med funktionen Stäng av visas inte dialog rutan fråga när Log Analytics skärm läses in.

![Exempel på av](media/saved-queries/examples-on-off.png)

Du kan alltid komma åt exempel frågans popup-miljö från knappen *exempel frågor* i det översta fältet i Log Analytics.

## <a name="query-explorer"></a>Query Explorer

Query Explorer-upplevelsen för att spara och dela användardefinierade frågor förblir oförändrad under tiden.

## <a name="next-steps"></a>Nästa steg

[Kom igång med KQL-frågor](get-started-queries.md)

