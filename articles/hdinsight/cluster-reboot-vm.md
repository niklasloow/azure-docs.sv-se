---
title: Starta om virtuella datorer för Azure HDInsight-kluster
description: Lär dig hur du startar om virtuella datorer som inte svarar för Azure HDInsight-kluster.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.custom: hdinsightactive
ms.service: hdinsight
ms.topic: how-to
ms.date: 06/22/2020
ms.openlocfilehash: 149a82526263f5e372db81b5a92a9ee90a2c76f3
ms.sourcegitcommit: 07166a1ff8bd23f5e1c49d4fd12badbca5ebd19c
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 09/15/2020
ms.locfileid: "90089980"
---
# <a name="reboot-vms-for-hdinsight-clusters"></a>Starta om virtuella datorer för HDInsight-kluster

Azure HDInsight-kluster innehåller grupper med virtuella datorer (VM) som klusternoder. För tids krävande kluster kan de här noderna sluta fungera av olika orsaker. I den här artikeln beskrivs hur du startar om virtuella datorer som inte svarar i ett HDInsight-kluster.

## <a name="when-to-reboot"></a>När du ska starta om

> [!WARNING]
> När du startar om virtuella datorer i ett kluster är noden inte tillgänglig för användning och tjänsterna på noden måste startas om.

När en nod startas om kan klustret bli ohälsosamt, och jobben kan bli långsamma eller misslyckade. Om du försöker starta om den aktiva Head-noden stoppas alla jobb som körs. Du kommer inte att kunna skicka jobb till klustret förrän tjänsterna är igång och körs igen. Av dessa skäl bör du starta om virtuella datorer endast när det behövs. Överväg att starta om virtuella datorer när:

- Du kan inte använda SSH för att komma till noden, men den svarar på pingar.
- Arbetsnoden är nere utan pulsslag i Ambari-ANVÄNDARGRÄNSSNITTET.
- Den temporära disken är full på noden.
- Process tabellen på den virtuella datorn har många poster där processen har slutförts, men den visas med "avslutad status".

> [!WARNING]
> Var försiktig när du startar om virtuella datorer för **HBase** -och **Kafka** -kluster eftersom omstart kan leda till att data går förlorade.

## <a name="use-powershell-to-reboot-vms"></a>Använd PowerShell för att starta om virtuella datorer

Det krävs två steg för att använda åtgärden för att starta om noden: lista noder och starta om noder.

1. Lista noder. Du kan hämta listan över klusternoder på [Get-AzHDInsightHost](https://docs.microsoft.com/powershell/module/az.hdinsight/get-azhdinsighthost).

      ```
      Get-AzHDInsightHost -ClusterName myclustername
      ```

1. Starta om värdar. När du får namnen på de noder som du vill starta om startar du om noderna med hjälp av [restart-AzHDInsightHost](https://docs.microsoft.com/powershell/module/az.hdinsight/restart-azhdinsighthost).

      ```
      Restart-AzHDInsightHost -ClusterName myclustername -Name wn0-myclus, wn1-myclus
      ```

## <a name="use-a-rest-api-to-reboot-vms"></a>Använda en REST API för att starta om virtuella datorer

Du kan använda funktionen **testa IT** i API-dokumentet för att skicka begär anden till HDInsight. Det krävs två steg för att använda åtgärden för att starta om noden: lista noder och starta om noder.

1. Lista noder. Du kan hämta listan över klusternoder från REST API eller i Ambari. Mer information finns i [HDInsight List hosts REST API operation](https://docs.microsoft.com/rest/api/hdinsight/virtualmachines/listhosts).

    ```
    POST https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.HDInsight/clusters/{clusterName}/listHosts?api-version=2018-06-01-preview
    ```

1. Starta om värdar. När du har skaffat namnen på de noder som du vill starta om startar du om noderna med hjälp av REST API för att starta om noderna. Nodnamn följer mönstret för *NodeType (ned/HN/ZK/GW)*  +  *x*  +  *första sex tecken i kluster namn*. Mer information finns i [HDInsight restart hosts REST API operation](https://docs.microsoft.com/rest/api/hdinsight/virtualmachines/restarthosts).

    ```
    POST https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.HDInsight/clusters/{clusterName}/restartHosts?api-version=2018-06-01-preview
    ```

De faktiska namnen på de noder som du vill starta om anges i en JSON-matris i begär ande texten.

```json
[
  "wn0-abcdef",
  "zk1-abcdef"
]
```

## <a name="next-steps"></a>Nästa steg

* [Starta om AzHDInsightHost](https://docs.microsoft.com/powershell/module/az.hdinsight/restart-azhdinsighthost)
* [Virtuella HDInsight-datorer REST API](https://docs.microsoft.com/rest/api/hdinsight/virtualmachines)
* [HDInsight-REST API](https://docs.microsoft.com/rest/api/hdinsight/)
