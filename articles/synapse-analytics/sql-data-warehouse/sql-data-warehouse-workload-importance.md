---
title: Arbetsbelastningsprioritet
description: Vägledning för att ställa in prioritet för Synapse i SQL-pooler i Azure Synapse Analytics.
services: synapse-analytics
author: ronortloff
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: sql-dw
ms.date: 02/04/2020
ms.author: rortloff
ms.reviewer: jrasnick
ms.custom: azure-synapse
ms.openlocfilehash: 1b2c71d7bf9e796af77e9a2a4a3a31152f2ca884
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/02/2020
ms.locfileid: "85212351"
---
# <a name="azure-synapse-analytics-workload-importance"></a>Arbets belastnings prioritet för Azure Synapse Analytics

I den här artikeln förklaras hur arbets belastnings prioriteten kan påverka ordningen för körningen av Synapse för SQL-pooler i Azure Synapse.

## <a name="importance"></a>Betydelse

> [!Video https://www.youtube.com/embed/_2rLMljOjw8]

Affärs behov kan kräva att data lager arbets belastningar är viktigare än andra.  Överväg ett scenario där verksamhets kritiska försäljnings data läses in innan räkenskaps perioden stängs.  Data inläsningar för andra källor, t. ex. väder data, har ingen strikt service avtal. Att ställa in hög prioritet för en begäran om att läsa in försäljnings data och låg prioritet för en begäran om att läsa in väder data, säkerställer att försäljnings data inläsningen först får till gång till resurser och slutför snabbare.

## <a name="importance-levels"></a>Prioritets nivåer

Det finns fem viktiga nivåer: låg, below_normal, normal, above_normal och hög.  Begär Anden som inte anger prioritet tilldelas standard nivån normal. Förfrågningar som har samma prioritets nivå har samma schemaläggnings beteende som finns idag.

## <a name="importance-scenarios"></a>Viktiga scenarier

Utöver det grundläggande prioritets scenario som beskrivs ovan med försäljnings-och väder data finns det andra scenarier där arbets belastnings prioriteten hjälper till att möta data bearbetning och frågor om behov.

### <a name="locking"></a>Låsning

Åtkomst till Lås för läsnings-och skriv aktivitet är ett av naturliga konkurrens områden. Aktiviteter som [partitions växling](sql-data-warehouse-tables-partition.md) eller [namnbytes objekt](/sql/t-sql/statements/rename-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest) kräver utökade lås.  Utan arbets belastnings prioritet optimerar Synapse SQL-poolen i Azure Synapse för data flödet. Optimering för data flöde innebär att när du kör och köade begär Anden har samma lås behov och resurser är tillgängliga, kan köade begär Anden kringgå begär Anden med högre låsnings behov som anlänt i kön för begär Anden tidigare. När arbets belastnings prioriteten tillämpas på begär Anden med högre låsnings behov. Begäran med högre prioritet kommer att köras före begäran med lägre prioritet.

Se följande exempel:

- Q1 körs aktivt och väljer data från SalesFact.
- K2 i kö väntar på att Q1 ska slutföras.  Den skickades vid 9 och försöker att byta ny data till SalesFact.
- Q3 skickas kl. 9:01am och vill välja data från SalesFact.

Om Q2 och Q3 har samma betydelse och Q1 fortfarande körs, börjar Q3 att köras. Q2 fortsätter att vänta på ett exklusivt lås på SalesFact.  Om Q2 har högre prioritet än Q3, kommer Q3 att vänta tills K2 är klart innan den kan börja köras.

### <a name="non-uniform-requests"></a>Icke-enhetliga begär Anden

Ett annat scenario där prioriteten kan hjälpa till att möta frågor om krav är när förfrågningar med olika resurs klasser skickas.  Som tidigare nämnts har Synapse SQL-poolen i Azure Synapse optimerats för data flödet. När blandade storleks begär Anden (till exempel smallrc eller mediumrc) placeras i kö, kommer Synapse SQL-pool att välja den tidigaste inkommande begäran som passar i de tillgängliga resurserna. Om prioriteten för arbets belastningen används schemaläggs den högsta prioriteten nästa gång.
  
Tänk på följande exempel på DW500c:

- Q1, Q2, Q3 och Q4 kör smallrc-frågor.
- Q5 skickas med resurs klassen mediumrc vid 9.
- Q6 skickas med smallrc resurs klass vid 9:01am.

Eftersom Q5 är mediumrc krävs två samtidiga platser. Q5 måste vänta på att två av de körnings bara frågorna ska slutföras.  Men när en av frågorna som körs (Q1-Q4) är klar, schemaläggs Q6 omedelbart, eftersom det finns resurser för att köra frågan.  Om Q5 har högre prioritet än Q6, väntar Q6 tills Q5 körs innan den kan börja köra.

## <a name="next-steps"></a>Nästa steg

- Mer information om hur du skapar en klassificerare finns i [create klassificerare för arbets belastning (Transact-SQL)](/sql/t-sql/statements/create-workload-classifier-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest).  
- Mer information om arbets belastnings klassificering finns i avsnittet om [arbets belastnings klassificering](sql-data-warehouse-workload-classification.md).  
- Se hur du skapar en arbets belastnings klassificering genom att [skapa](quickstart-create-a-workload-classifier-tsql.md) en arbets belastnings klassificerare för snabb start.
- Se instruktions artiklar för att [Konfigurera arbets belastnings prioritet](sql-data-warehouse-how-to-configure-workload-importance.md) och hur du [hanterar och övervakar arbets belastnings hantering](sql-data-warehouse-how-to-manage-and-monitor-workload-importance.md).
- Se [sys. dm_pdw_exec_requests](/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-exec-requests-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest) för att visa frågor och prioriteten som tilldelats.
