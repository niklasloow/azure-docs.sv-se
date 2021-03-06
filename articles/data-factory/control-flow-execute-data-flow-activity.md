---
title: Data flödes aktivitet
description: Så här kör du data flöden inifrån en Data Factory-pipeline.
services: data-factory
documentationcenter: ''
author: kromerm
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.author: makromer
ms.date: 04/30/2020
ms.openlocfilehash: 5593b0d633b133c8a8295634b674218d5e6c6daf
ms.sourcegitcommit: de2750163a601aae0c28506ba32be067e0068c0c
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 09/04/2020
ms.locfileid: "89485045"
---
# <a name="data-flow-activity-in-azure-data-factory"></a>Data flödes aktivitet i Azure Data Factory

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Använd data flödes aktiviteten för att transformera och flytta data via data flöden för mappning. Om du är nybörjare på data flöden, se [Översikt över kart data flöde](concepts-data-flow-overview.md)

## <a name="syntax"></a>Syntax

```json
{
    "name": "MyDataFlowActivity",
    "type": "ExecuteDataFlow",
    "typeProperties": {
      "dataflow": {
         "referenceName": "MyDataFlow",
         "type": "DataFlowReference"
      },
      "compute": {
         "coreCount": 8,
         "computeType": "General"
      },
      "staging": {
          "linkedService": {
              "referenceName": "MyStagingLinkedService",
              "type": "LinkedServiceReference"
          },
          "folderPath": "my-container/my-folder"
      },
      "integrationRuntime": {
          "referenceName": "MyDataFlowIntegrationRuntime",
          "type": "IntegrationRuntimeReference"
      }
}

```

## <a name="type-properties"></a>Typ egenskaper

Egenskap | Beskrivning | Tillåtna värden | Obligatorisk
-------- | ----------- | -------------- | --------
dataflöde | Referens till det data flöde som körs | DataFlowReference | Ja
integrationRuntime | Beräknings miljön som data flödet körs på. Om inget anges används automatisk lösning för Azure integration Runtime. | IntegrationRuntimeReference | Inga
Compute. coreCount | Antalet kärnor som används i Spark-klustret. Kan bara anges om automatisk matchning av Azure integration runtime används | 8, 16, 32, 48, 80, 144, 272 | Inga
Compute. computeType | Den typ av beräkning som används i Spark-klustret. Kan bara anges om automatisk matchning av Azure integration runtime används | "Allmänt", "ComputeOptimized", "MemoryOptimized" | Inga
mellanlagring. linkedService | Om du använder en Azure Synapse Analytics-källa eller-mottagare är det lagrings konto som används för PolyBase-mellanlagring | LinkedServiceReference | Endast om data flödet läser eller skriver till en Azure Synapse-analys
mellanlagring. folderPath | Om du använder en Azure Synapse Analytics-källa eller handfat, används mappsökvägen i Blob Storage-kontot för PolyBase-mellanlagring | Sträng | Endast om data flödet läser eller skriver till Azure Synapse Analytics

![Kör data flöde](media/data-flow/activity-data-flow.png "Kör data flöde")

### <a name="dynamically-size-data-flow-compute-at-runtime"></a>Data flödes beräkning dynamiskt vid körning

Egenskaperna Core count och Compute Type kan ställas in dynamiskt så att de anpassas till storleken på dina inkommande källdata vid körning. Använd pipeline-aktiviteter som lookup eller get metadata för att hitta storleken på käll data uppsättningens data. Använd sedan Lägg till dynamiskt innehåll i egenskaperna för data flödes aktiviteten.

![Dynamiskt data flöde](media/data-flow/dyna1.png "Dynamiskt data flöde")

[Här är en kort video kurs som förklarar den här tekniken](https://www.youtube.com/watch?v=jWSkJdtiJNM)

### <a name="data-flow-integration-runtime"></a>Integration runtime för data flöde

Välj vilken Integration Runtime som ska användas för körningen av data flödes aktiviteten. Som standard använder Data Factory automatisk lösning för Azure integration runtime med fyra arbets kärnor och inget TTL-värde (Time to Live). Denna IR har en generell beräknings typ och körs i samma region som din fabrik. Du kan skapa dina egna Azure-integrerings körningar som definierar specifika regioner, beräknings typ, antal kärnor och TTL för din data flödes aktivitets körning.

För pipeline-körningar är klustret ett jobb kluster, vilket tar flera minuter att starta innan körningen börjar. Om du inte anger något TTL-värde krävs den här start tiden för varje pipeline-körning. Om du anger ett TTL-värde förblir en varm klustrad pool aktiv under den tid som anges efter den senaste körningen, vilket leder till kortare start tider. Om du till exempel har en TTL på 60 minuter och kör ett data flöde på den en gång i timmen förblir anslutningspoolen aktiv. Mer information finns i [Azure integration runtime](concepts-integration-runtime.md).

![Azure Integration Runtime](media/data-flow/ir-new.png "Azure Integration Runtime")

> [!IMPORTANT]
> Integration Runtime valet i data flödes aktiviteten gäller endast *utlösta körningar* av din pipeline. Fel sökning av din pipeline med data flöden körs på det kluster som anges i felsökningssessionen.

### <a name="polybase"></a>PolyBase

Om du använder en Azure Synapse-analys (tidigare SQL Data Warehouse) som mottagare eller källa, måste du välja en mellanlagringsplats för grupp inläsningen för PolyBase. PolyBase tillåter satsvis inläsning i bulk i stället för att läsa in data rad för rad. PolyBase minskar drastiskt inläsnings tiden till Azure Synapse Analytics.

## <a name="parameterizing-data-flows"></a>Parameters-data flöden

### <a name="parameterized-datasets"></a>Parameterstyrda data uppsättningar

Om data flödet använder parametriserade data uppsättningar anger du parameter värden på fliken **Inställningar** .

![Kör data flödes parametrar](media/data-flow/params.png "Parametrar")

### <a name="parameterized-data-flows"></a>Parameter data flöden

Om ditt data flöde är parameterstyrda anger du de dynamiska värdena för data flödes parametrarna på fliken **parametrar** . Du kan använda antingen uttrycks språket för ADF-pipeline eller data flödes uttrycks språket för att tilldela dynamiska eller exakta parameter värden. Mer information finns i [data flödes parametrar](parameters-data-flow.md).

### <a name="parameterized-compute-properties"></a>Parameter beräknings egenskaper.

Du kan Parameterisera för antalet kärnor eller beräknings typen om du använder automatisk lösning för Azure integration Runtime och anger värden för Compute. coreCount och Compute. computeType.

![Exempel på körning av data flödes parameter](media/data-flow/parameterize-compute.png "Parameter exempel")

## <a name="pipeline-debug-of-data-flow-activity"></a>Fel sökning av pipeline för data flödes aktivitet

Om du vill köra en pipeline för fel sökning med en data flödes aktivitet måste du växla till fel söknings läge för data flöde via skjutreglaget för **fel sökning av data flöde** i det översta fältet. Med fel söknings läge kan du köra data flödet mot ett aktivt Spark-kluster. Mer information finns i [fel söknings läge](concepts-data-flow-debug-mode.md).

![Knappen Felsök](media/data-flow/debugbutton.png "Knappen Felsök")

Fel söknings pipelinen körs mot det aktiva fel söknings klustret, inte integrerings körnings miljön som anges i inställningarna för data flödes aktiviteten. Du kan välja beräknings miljö för fel sökning när du startar fel söknings läge.

## <a name="monitoring-the-data-flow-activity"></a>Övervaka data flödes aktiviteten

Data flödes aktiviteten har en särskild övervaknings upplevelse där du kan visa information om partitionering, fas tid och data härkomst. Öppna fönstret övervakning via glasögon-ikonen under **åtgärder**. Mer information finns i [övervaka data flöden](concepts-data-flow-monitoring.md).

### <a name="use-data-flow-activity-results-in-a-subsequent-activity"></a>Använd data flödes aktivitets resultat i en efterföljande aktivitet

Data flödes aktiviteten matar ut mått för antalet rader som skrivs till varje mottagare och rader läses från varje källa. De här resultaten returneras i `output` avsnittet i aktivitets körnings resultatet. De värden som returneras är i formatet under JSON.

``` json
{
    "runStatus": {
        "metrics": {
            "<your sink name1>": {
                "rowsWritten": <number of rows written>,
                "sinkProcessingTime": <sink processing time in ms>,
                "sources": {
                    "<your source name1>": {
                        "rowsRead": <number of rows read>
                    },
                    "<your source name2>": {
                        "rowsRead": <number of rows read>
                    },
                    ...
                }
            },
            "<your sink name2>": {
                ...
            },
            ...
        }
    }
}
```

Om du till exempel vill komma till antalet rader som skrivs till en mottagare med namnet "sink1" i en aktivitet med namnet "dataflowActivity" använder du `@activity('dataflowActivity').output.runStatus.metrics.sink1.rowsWritten` .

Om du vill hämta antalet rader från en källa med namnet "source1" som användes i denna mottagare använder du `@activity('dataflowActivity').output.runStatus.metrics.sink1.sources.source1.rowsRead` .

> [!NOTE]
> Om en Sink har noll rader skrivna visas den inte i mått. Förekomst kan verifieras med hjälp av `contains` funktionen. Kontrollerar till exempel `contains(activity('dataflowActivity').output.runStatus.metrics, 'sink1')` om några rader skrevs till sink1.

## <a name="next-steps"></a>Nästa steg

Se kontroll flödes aktiviteter som stöds av Data Factory: 

- [If-villkorsaktivitet](control-flow-if-condition-activity.md)
- [Köra pipelineaktivitet](control-flow-execute-pipeline-activity.md)
- [För varje aktivitet](control-flow-for-each-activity.md)
- [Hämta metadataaktivitet](control-flow-get-metadata-activity.md)
- [Sökningsaktivitet](control-flow-lookup-activity.md)
- [Webbaktivitet](control-flow-web-activity.md)
- [Tills-aktivitet](control-flow-until-activity.md)
