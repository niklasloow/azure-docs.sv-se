---
title: Felsöka skript åtgärder i Azure HDInsight
description: Allmänna fel söknings steg för skript åtgärder i Azure HDInsight.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: troubleshooting
ms.custom: seoapr2020
ms.date: 04/21/2020
ms.openlocfilehash: e8585779a263f4ff5dbdd998bbf065c6a4e1acdf
ms.sourcegitcommit: 124f7f699b6a43314e63af0101cd788db995d1cb
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/08/2020
ms.locfileid: "86079268"
---
# <a name="troubleshoot-script-actions-in-azure-hdinsight"></a>Felsöka skript åtgärder i Azure HDInsight

Den här artikeln beskriver fel söknings steg och möjliga lösningar för problem med att interagera med Azure HDInsight-kluster.

## <a name="viewing-logs"></a>Visa loggar

Du kan använda Apache Ambari Web UI för att visa information som loggats av skript åtgärder. Om skriptet Miss lyckas när klustret skapas finns loggar i standard klustrets lagrings konto. Det här avsnittet innehåller information om hur du hämtar loggarna med båda dessa alternativ.

### <a name="apache-ambari-web-ui"></a>Apache Ambari-webbgränssnitt

1. I en webbläsare går du till `https://CLUSTERNAME.azurehdinsight.net` , där `CLUSTERNAME` är namnet på klustret.

1. Välj **Ops** -posten från fältet överst på sidan. En lista visar aktuella och tidigare åtgärder som utförs på klustret via Ambari.

    ![Ambari Web UI-fältet med OPS valt](./media/troubleshoot-script-action/hdi-apache-ambari-nav.png)

1. Hitta de poster som har **kört \_ customscriptaction** i kolumnen **åtgärder** . Dessa poster skapas när skript åtgärderna körs.

    ![Åtgärds åtgärder för Apache Ambari-skript](./media/troubleshoot-script-action/ambari-script-action.png)

    Om du vill visa **STDOUT** -och **stderr** -utdata väljer du posten **run\customscriptaction** och ökar detalj nivån genom länkarna. Dessa utdata skapas när skriptet körs och kan ha användbar information.

### <a name="default-storage-account"></a>Standard lagrings konto

Om det inte går att skapa ett kluster på grund av ett skript fel sparas loggarna i klustrets lagrings konto.

* Lagrings loggarna är tillgängliga på `\STORAGE_ACCOUNT_NAME\DEFAULT_CONTAINER_NAME\custom-scriptaction-logs\CLUSTER_NAME\DATE` .

    ![Skript åtgärds loggar](./media/troubleshoot-script-action/script-action-logs-in-storage.png)

    Under den här katalogen ordnas loggarna separat för **huvudnoden**, **arbetsnoden**och **Zookeeper-noden**. Se följande exempel:

    * **Huvudnoden**:`<ACTIVE-HEADNODE-NAME>.cloudapp.net`

    * **Arbets nod**:`<ACTIVE-WORKERNODE-NAME>.cloudapp.net`

    * **Zookeeper-nod**:`<ACTIVE-ZOOKEEPERNODE-NAME>.cloudapp.net`

* Alla **STDOUT** och **stderr** för motsvarande värd överförs till lagrings kontot. Det finns en **output- \* . txt** och **errors- \* . txt** för varje skript åtgärd. Filen **output-*. txt** innehåller information om URI: n för skriptet som kördes på värden. Följande text är ett exempel på den här informationen:

    ```output
    'Start downloading script locally: ', u'https://hdiconfigactions.blob.core.windows.net/linuxrconfigactionv01/r-installer-v01.sh'
    ```

* Det är möjligt att du upprepade gånger skapar ett skript åtgärds kluster med samma namn. I så fall kan du särskilja relevanta loggar baserat på namnet på mappen för **datum** . Mappstrukturen för ett **kluster, till exempel, som**skapas på olika datum ser ut ungefär som i följande logg poster:

    `\STORAGE_ACCOUNT_NAME\DEFAULT_CONTAINER_NAME\custom-scriptaction-logs\mycluster\2015-10-04` `\STORAGE_ACCOUNT_NAME\DEFAULT_CONTAINER_NAME\custom-scriptaction-logs\mycluster\2015-10-05`

* Om du skapar ett skript åtgärds kluster med samma namn samma dag kan du använda det unika prefixet för att identifiera relevanta loggfiler.

* Om du skapar ett kluster nära 12:00 AM, midnatt, är det möjligt att loggfilerna sträcker sig över två dagar. I så fall visas två olika date-mappar för samma kluster.

* Det kan ta upp till fem minuter att ladda upp loggfiler till standard behållaren, särskilt för stora kluster. Så om du vill komma åt loggarna bör du inte omedelbart ta bort klustret om en skript åtgärd Miss lyckas.

## <a name="ambari-watchdog"></a>Ambari övervaknings enhet

Ändra inte lösen ordet för Ambari-övervaknings enheten, hdinsightwatchdog, på ditt Linux-baserade HDInsight-kluster. En lösen ords ändring bryter möjligheten att köra nya skript åtgärder i HDInsight-klustret.

## <a name="cant-import-name-blobservice"></a>Det går inte att importera namnet BlobService

__Symptom__. Skript åtgärden Miss lyckas. Text som liknar följande fel visas när du visar åtgärden i Ambari:

```
Traceback (most recent call list):
  File "/var/lib/ambari-agent/cache/custom_actions/scripts/run_customscriptaction.py", line 21, in <module>
    from azure.storage.blob import BlobService
ImportError: cannot import name BlobService
```

__Orsak__. Det här felet uppstår om du uppgraderar python-Azure Storage-klienten som ingår i HDInsight-klustret. HDInsight förväntar sig Azure Storage 0.20.0-klient.

__Lösning__. För att lösa det här felet ansluter du manuellt till varje klusternod med hjälp av `ssh` . Kör följande kommando för att installera om rätt version av lagrings klienten:

```bash
sudo pip install azure-storage==0.20.0
```

Information om hur du ansluter till klustret med SSH finns i [ansluta till HDInsight (Apache Hadoop) med hjälp av SSH](hdinsight-hadoop-linux-use-ssh-unix.md).

## <a name="history-doesnt-show-the-scripts-used-during-cluster-creation"></a>Historik visar inte de skript som används när klustret skapas

Om klustret skapades före den 15 mars 2016 kanske du inte ser någon post i åtgärds historiken för skriptet. Att ändra storlek på klustret gör att skripten visas i skript åtgärds historik.

Det finns två undantag:

* Klustret skapades före den 1 september 2015. Det här datumet är när skript åtgärder introducerades. Alla kluster som skapats före det här datumet kunde inte använda skript åtgärder för att skapa kluster.

* Du har använt flera skript åtgärder under skapandet av klustret. Eller så har du använt samma namn för flera skript eller samma namn, samma URI, men olika parametrar för flera skript. I dessa fall får du följande fel meddelande:

    ```
    No new script actions can be run on this cluster because of conflicting script names in existing scripts. Script names provided at cluster creation must be all unique. Existing scripts are run on resize.
    ```

## <a name="next-steps"></a>Nästa steg

Om du inte ser problemet eller inte kan lösa problemet kan du gå till någon av följande kanaler för mer support:

* Få svar från Azure-experter via [Azure community support](https://azure.microsoft.com/support/community/).

* Anslut till [@AzureSupport](https://twitter.com/azuresupport) – det officiella Microsoft Azure kontot för att förbättra kund upplevelsen. Att ansluta Azure-communityn till rätt resurser: svar, support och experter.

* Om du behöver mer hjälp kan du skicka en support förfrågan från [Azure Portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/). Välj **stöd** på Meny raden eller öppna **Hjälp + Support** Hub. Mer detaljerad information finns [i så här skapar du en support förfrågan för Azure](https://docs.microsoft.com/azure/azure-portal/supportability/how-to-create-azure-support-request). Åtkomst till prenumerations hantering och fakturerings support ingår i din Microsoft Azure prenumeration och teknisk support tillhandahålls via ett av support avtalen för [Azure](https://azure.microsoft.com/support/plans/).
