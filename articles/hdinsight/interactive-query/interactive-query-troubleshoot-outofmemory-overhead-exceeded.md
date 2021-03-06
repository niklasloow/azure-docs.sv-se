---
title: Kopplingar i Apache Hive leder till OutOfMemory-fel – Azure HDInsight
description: Hantering av OutOfMemory fel meddelandet "GC-overhead-gräns överskreds"
ms.service: hdinsight
ms.topic: troubleshooting
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.date: 07/30/2019
ms.openlocfilehash: ab334dfb15044fd0734a107c12003ca2c1f86906
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/02/2020
ms.locfileid: "75895177"
---
# <a name="scenario-joins-in-apache-hive-leads-to-an-outofmemory-error-in-azure-hdinsight"></a>Scenario: kopplingar i Apache Hive leder till ett OutOfMemory-fel i Azure HDInsight

I den här artikeln beskrivs fel söknings steg och möjliga lösningar på problem vid användning av interaktiva frågekomponenter i Azure HDInsight-kluster.

## <a name="issue"></a>Problem

Standard beteendet för Apache Hive kopplingar är att läsa in hela innehållet i en tabell i minnet så att en koppling kan utföras utan att du behöver utföra ett kart-/minsknings steg. Om Hive-tabellen är för stor för att få plats i minnet kan frågan inte köras.

## <a name="cause"></a>Orsak

När du kör kopplingar i Hive med tillräcklig storlek, påträffas följande fel:

```
Caused by: java.lang.OutOfMemoryError: GC overhead limit exceeded error.
```

## <a name="resolution"></a>Lösning

Förhindra att Hive läser in tabeller i minnet i kopplingar (i stället utföra ett kart-/minsknings steg) genom att ange följande konfigurations värde för Hive:

```
hive.auto.convert.join=false
```

## <a name="next-steps"></a>Nästa steg

Om du inte kunde lösa problemet med det här värdet kan du gå till något av följande...

* Få svar från Azure-experter via [Azure community support](https://azure.microsoft.com/support/community/).

* Anslut till [@AzureSupport](https://twitter.com/azuresupport) – det officiella Microsoft Azure kontot för att förbättra kund upplevelsen genom att ansluta Azure-communityn till rätt resurser: svar, support och experter.

* Om du behöver mer hjälp kan du skicka en support förfrågan från [Azure Portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/). Välj **stöd** på Meny raden eller öppna **Hjälp + Support** Hub. Mer detaljerad information finns [i så här skapar du en support förfrågan för Azure](https://docs.microsoft.com/azure/azure-portal/supportability/how-to-create-azure-support-request). Åtkomst till prenumerations hantering och fakturerings support ingår i din Microsoft Azure prenumeration och teknisk support tillhandahålls via ett av support avtalen för [Azure](https://azure.microsoft.com/support/plans/).
