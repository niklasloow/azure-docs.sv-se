---
title: Metod tips för SQL på begäran (för hands version)
description: Rekommendationer och metod tips som du bör känna till när du arbetar med SQL på begäran (för hands version).
services: synapse-analytics
author: filippopovic
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: sql
ms.date: 05/01/2020
ms.author: fipopovi
ms.reviewer: jrasnick
ms.openlocfilehash: 7bebfeba6da1493557d51777ba8438747e160750
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/02/2020
ms.locfileid: "85476282"
---
# <a name="best-practices-for-sql-on-demand-preview-in-azure-synapse-analytics"></a>Metod tips för SQL på begäran (för hands version) i Azure Synapse Analytics

I den här artikeln hittar du en samling bästa metoder för att använda SQL på begäran (för hands version). SQL på begäran är en resurs i Azure Synapse Analytics.

## <a name="general-considerations"></a>Generella saker att tänka på

Med SQL på begäran kan du söka efter filer i dina Azure Storage-konton. Den har inte funktioner för lokal lagring eller inmatning. Så alla filer som fråge målen är externa för SQL på begäran. Allt som rör läsning av filer från lagring kan påverka frågans prestanda.

## <a name="colocate-your-azure-storage-account-and-sql-on-demand"></a>Samplacera ditt Azure Storage-konto och SQL på begäran

Du kan minimera svars tiden genom att samplacera ditt Azure Storage-konto och din SQL-slutpunkt på begäran. Lagrings konton och slut punkter som tillhandahålls när arbets ytan skapas befinner sig i samma region.

För optimala prestanda bör du kontrol lera att de finns i samma region om du har åtkomst till andra lagrings konton med SQL på begäran. Om de inte finns i samma region ökar svars tiden för data överföring mellan den fjärranslutna regionen och slut punktens region.

## <a name="azure-storage-throttling"></a>Azure Storage begränsning

Flera program och tjänster kan komma åt ditt lagrings konto. Lagrings begränsning sker när den sammanlagda IOPS eller data flödet som genereras av program, tjänster och SQL på begäran-arbetsbelastning överskrider lagrings kontots gränser. Därför får du en betydande negativ effekt på frågans prestanda.

När begränsningen har identifierats har SQL på begäran inbyggd hantering för att lösa det. SQL på begäran kommer att göra begär anden till lagringen i en långsammare takt tills begränsningen har lösts.

> [!TIP]
> För optimal frågekörning ska du inte stressa lagrings kontot med andra arbets belastningar under frågekörningen.

## <a name="prepare-files-for-querying"></a>Förbered filer för frågor

Om möjligt kan du förbereda filer för bättre prestanda:

- Konvertera CSV och JSON till Parquet. Parquet är ett kolumn format. Eftersom fil storleken är komprimerad är fil storleken mindre än CSV eller JSON-filer som innehåller samma data. SQL på begäran behöver mindre tid och färre lagrings begär Anden att läsa.
- Om en fråga är riktad mot en enda stor fil, drar du nytta av att dela upp den i flera mindre filer.
- Försök att behålla storleken på CSV-filen under 10 GB.
- Det är bättre att ha lika stora filer för en enskild OpenRowSet-sökväg eller en extern tabell plats.
- Partitionera dina data genom att lagra partitioner i olika mappar eller fil namn. Se [använda fil namns-och fil Sök vägar för att fokusera på specifika partitioner](#use-filename-and-filepath-functions-to-target-specific-partitions).

## <a name="push-wildcards-to-lower-levels-in-the-path"></a>Jokertecken för push-meddelanden till lägre nivåer i sökvägen

Du kan använda jokertecken i din sökväg för att [fråga flera filer och mappar](query-data-storage.md#query-multiple-files-or-folders). SQL på begäran listar filer på ditt lagrings konto, från och med den första * använda Storage API. Den eliminerar filer som inte matchar den angivna sökvägen. Att minska den inledande listan över filer kan förbättra prestanda om det finns många filer som matchar den angivna sökvägen upp till det första jokertecknet.

## <a name="use-appropriate-data-types"></a>Använd lämpliga data typer

De data typer som du använder i frågan påverkar prestanda. Du kan få bättre prestanda om du följer dessa rikt linjer: 

- Använd den minsta data storlek som ska hantera det största möjliga värdet.
  - Om den maximala tecken längden är 30 tecken, använder du tecken data typen 30.
  - Om alla tecken kolumn värden har fast storlek använder du **char** eller **nchar**. Annars använder du **varchar** eller **nvarchar**.
  - Om värdet för heltals kolumn är 500 använder du **smallint** eftersom det är den minsta data typen som kan hantera det här värdet. Du kan hitta data typs intervall av typen Integer i [den här artikeln](https://docs.microsoft.com/sql/t-sql/data-types/int-bigint-smallint-and-tinyint-transact-sql?view=sql-server-ver15).
- Använd om möjligt **varchar** och **char** i stället för **nvarchar** och **nchar**.
- Använd Integer-baserade data typer om möjligt. Åtgärder för att sortera, ansluta och gruppera efter slutförs snabbare med heltal än på Character data.
- Om du använder schema härledning, [kontrollerar du härledda data typer](#check-inferred-data-types).

## <a name="check-inferred-data-types"></a>Kontrol lera härledda data typer

[Schema härledning](query-parquet-files.md#automatic-schema-inference) hjälper dig att snabbt skriva frågor och utforska data utan att känna till fil scheman. Kostnaden för den här bekvämligheten är att härledda data typer är större än de faktiska data typerna. Detta inträffar när det inte finns tillräckligt med information i källfilerna för att se till att rätt datatyp används. Parquet-filer innehåller till exempel inte metadata om maximal tecken kolumn längd. Så att SQL på begäran härleds som varchar (8000).

Du kan använda [sp_describe_first_results_set](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-describe-first-result-set-transact-sql?view=sql-server-ver15) för att kontrol lera de resulterande data typerna i din fråga.

I följande exempel visas hur du kan optimera härledda data typer. Den här proceduren används för att Visa härledda data typer: 
```sql  
EXEC sp_describe_first_result_set N'
    SELECT
        vendor_id, pickup_datetime, passenger_count
    FROM 
        OPENROWSET(
            BULK ''https://sqlondemandstorage.blob.core.windows.net/parquet/taxi/*/*/*'',
            FORMAT=''PARQUET''
        ) AS nyc';
```

Här är resultat uppsättningen:

|is_hidden|column_ordinal|name|system_type_name|max_length|
|----------------|---------------------|----------|--------------------|-------------------||
|0|1|vendor_id|varchar (8000)|8000|
|0|2|pickup_datetime|datetime2 (7)|8|
|0|3|passenger_count|int|4|

När du känner till de härledda data typerna för frågan kan du ange lämpliga data typer:

```sql  
SELECT
    vendor_id, pickup_datetime, passenger_count
FROM 
    OPENROWSET(
        BULK 'https://sqlondemandstorage.blob.core.windows.net/parquet/taxi/*/*/*',
        FORMAT='PARQUET'
    ) 
    WITH (
        vendor_id varchar(4), -- we used length of 4 instead of the inferred 8000
        pickup_datetime datetime2,
        passenger_count int
    ) AS nyc;
```

## <a name="use-filename-and-filepath-functions-to-target-specific-partitions"></a>Använd filename-och filename-funktioner för att fokusera på specifika partitioner

Data är ofta ordnade i partitioner. Du kan instruera SQL på begäran att fråga specifika mappar och filer. Då minskas antalet filer och mängden data som frågan behöver läsa och bearbeta. En extra bonus är att du får bättre prestanda.

Mer information finns i avsnittet om [fil namn](query-data-storage.md#filename-function) och fil [Sök väg](query-data-storage.md#filepath-function) och se exemplen för att [fråga efter vissa filer](query-specific-files.md).

> [!TIP]
> Skicka alltid resultatet av sökvägen och fil namns funktionerna till lämpliga data typer. Om du använder tecken data typer måste du se till att du använder rätt längd.

> [!NOTE]
> Funktioner som används för partitions Eli minering, sökväg och fil namn stöds för närvarande inte för externa tabeller, förutom de som skapats automatiskt för varje tabell som skapats i Apache Spark för Azure Synapse Analytics.

Om dina lagrade data inte är partitionerade kan du partitionera dem. På så sätt kan du använda dessa funktioner för att optimera frågor som riktar sig mot dessa filer. När du [frågar partitionerade Apache Spark för Azure Synapse-tabeller](develop-storage-files-spark-tables.md) från SQL på begäran, kommer frågan automatiskt att rikta in sig på de nödvändiga filerna.

## <a name="use-parser_version-20-to-query-csv-files"></a>Använd PARSER_VERSION 2,0 för att fråga CSV-filer

Du kan använda en Prestandaoptimerad parser när du frågar CSV-filer. Mer information finns i [PARSER_VERSION](develop-openrowset.md).

## <a name="use-cetas-to-enhance-query-performance-and-joins"></a>Använd CETAS för att förbättra frågornas prestanda och kopplingar

[CETAS](develop-tables-cetas.md) är en av de viktigaste funktionerna som är tillgängliga i SQL på begäran. CETAS är en parallell åtgärd som skapar externa tabellens metadata och exporterar URVALs resultatet till en uppsättning filer i ditt lagrings konto.

Du kan använda CETAS för att lagra ofta använda delar av frågor som kopplade referens tabeller till en ny uppsättning filer. Du kan sedan ansluta till den här enskilda externa tabellen i stället för att upprepa vanliga kopplingar i flera frågor.

När CETAS genererar Parquet-filer skapas statistik automatiskt när den första frågan riktar sig till den externa tabellen, vilket resulterar i förbättrade prestanda.

## <a name="azure-ad-pass-through-performance"></a>Prestanda för Azure AD-vidarekoppling

Med SQL på begäran kan du komma åt filer i lagringen med hjälp av Azure Active Directory (Azure AD) genom strömnings-eller SAS-autentiseringsuppgifter. Du kan uppleva sämre prestanda med Azure AD genom strömning än med SAS.

Om du behöver bättre prestanda kan du försöka använda SAS-autentiseringsuppgifter för att komma åt lagrings utrymmet tills Azure AD-vidarekoppling har förbättrats.

## <a name="next-steps"></a>Nästa steg

Läs [fel söknings](../sql-data-warehouse/sql-data-warehouse-troubleshoot.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json) artikeln för lösningar på vanliga problem. Om du arbetar med SQL-pooler i stället för SQL på begäran kan du läsa mer i [metod tips för SQL-pooler](best-practices-sql-pool.md) .
