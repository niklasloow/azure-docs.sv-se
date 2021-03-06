---
title: Använda lagrade procedurer
description: Tips för att implementera lagrade procedurer i Synapse SQL för utveckling av lösningar.
services: synapse-analytics
author: XiaoyuMSFT
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: sql
ms.date: 09/23/2020
ms.author: xiaoyul
ms.reviewer: igorstan
ms.openlocfilehash: f2046614f3665a699d02c76210676fb32f99fc73
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 09/25/2020
ms.locfileid: "91288927"
---
# <a name="use-stored-procedures-in-synapse-sql"></a>Använda lagrade procedurer i Synapse SQL

Tips för att implementera lagrade procedurer i Synapse SQL-pool (Data Warehouse) för att utveckla lösningar.

## <a name="what-to-expect"></a>Vad du kan förvänta dig

Synapse SQL stöder många av de T-SQL-funktioner som används i SQL Server. Det är viktigt att det finns skalbara funktioner som du kan använda för att maximera prestandan för din lösning.

För att upprätthålla skalning och prestanda i SQL-poolen finns det också vissa funktioner och funktioner som har beteende skillnader och andra som inte stöds.

## <a name="stored-procedures-in-synapse-sql"></a>Lagrade procedurer i Synapse SQL

Lagrade procedurer är ett bra sätt att kapsla in din SQL-kod och lagra den nära dina data i data lagret. Lagrade procedurer hjälper utvecklare att modularize sina lösningar genom att kapsla in koden i hanterbara enheter, vilket underlättar större åter användning av kod. Varje lagrad procedur kan också acceptera parametrar för att göra dem ännu mer flexibla. I följande exempel kan du se de procedurer som släpper externa objekt om de finns i databasen:

```sql
CREATE PROCEDURE drop_external_table_if_exists @name SYSNAME
AS BEGIN
    IF (0 <> (SELECT COUNT(*) FROM sys.external_tables WHERE name = @name))
    BEGIN
        DECLARE @drop_stmt NVARCHAR(200) = N'DROP EXTERNAL TABLE ' + @name; 
        EXEC sp_executesql @tsql = @drop_stmt;
    END
END
GO
CREATE PROCEDURE drop_external_file_format_if_exists @name SYSNAME
AS BEGIN
    IF (0 <> (SELECT COUNT(*) FROM sys.external_file_formats WHERE name = @name))
    BEGIN
        DECLARE @drop_stmt NVARCHAR(200) = N'DROP EXTERNAL FILE FORMAT ' + @name; 
        EXEC sp_executesql @tsql = @drop_stmt;
    END
END
GO
CREATE PROCEDURE drop_external_data_source_if_exists @name SYSNAME
AS BEGIN
    IF (0 <> (SELECT COUNT(*) FROM sys.external_data_sources WHERE name = @name))
    BEGIN
        DECLARE @drop_stmt NVARCHAR(200) = N'DROP EXTERNAL DATA SOURCE ' + @name; 
        EXEC sp_executesql @tsql = @drop_stmt;
    END
END
```

Dessa procedurer kan utföras med hjälp av `EXEC` instruktioner där du kan ange procedur namn och parametrar:

```sql
EXEC drop_external_table_if_exists 'mytest';
EXEC drop_external_file_format_if_exists 'mytest';
EXEC drop_external_data_source_if_exists 'mytest';
```

Synapse SQL tillhandahåller en förenklad och strömlinjeformad implementering av lagrade procedurer. Den största skillnaden jämfört med SQL Server är att den lagrade proceduren inte är förkompilerad kod. I informations lager är kompileringen av tiden liten jämfört med den tid det tar att köra frågor mot stora data volymer. Det är viktigt att se till att den lagrade procedur koden är korrekt optimerad för stora frågor. Målet är att spara timmar, minuter och sekunder, inte millisekunder. Det är därför mer användbart att tänka på lagrade procedurer som behållare för SQL-logik.

När Synapse SQL kör den lagrade proceduren, parsas SQL-uttryck, översätts och optimeras vid körning. Under den här processen konverteras varje instruktion till distribuerade frågor. SQL-koden som körs mot data skiljer sig från den skickade frågan.

## <a name="encapsulate-validation-rules"></a>Kapsla in verifierings regler

Lagrade procedurer gör att du kan hitta validerings logik i en enda modul som lagras i SQL Database. I följande exempel kan du se hur du verifierar värdena för parametrar och ändrar standardvärdena.

```sql
CREATE PROCEDURE count_objects_by_date_created 
                            @start_date DATETIME2,
                            @end_date DATETIME2
AS BEGIN 

    IF( @start_date >= GETUTCDATE() )
    BEGIN
        THROW 51000, 'Invalid argument @start_date. Value should be in past.', 1;  
    END

    IF( @end_date IS NULL )
    BEGIN
        SET @end_date = GETUTCDATE();
    END

    IF( @start_date >= @end_date )
    BEGIN
        THROW 51000, 'Invalid argument @end_date. Value should be greater than @start_date.', 2;  
    END

    SELECT
         year = YEAR(create_date),
         month = MONTH(create_date),
         objects_created = COUNT(*)
    FROM
        sys.objects
    WHERE
        create_date BETWEEN @start_date AND @end_date
    GROUP BY
        YEAR(create_date), MONTH(create_date);
END
```

Logiken i SQL-proceduren verifierar indataparametrarna när proceduren anropas.

```sql

EXEC count_objects_by_date_created '2020-08-01', '2020-09-01'

EXEC count_objects_by_date_created '2020-08-01', NULL

EXEC count_objects_by_date_created '2020-09-01', '2020-08-01'
-- Error
-- Invalid argument @end_date. Value should be greater than @start_date.

EXEC count_objects_by_date_created '2120-09-01', NULL
-- Error
-- Invalid argument @start_date. Value should be in past.
```

## <a name="nesting-stored-procedures"></a>Kapslade lagrade procedurer

När lagrade procedurer anropar andra lagrade procedurer eller kör dynamisk SQL, kallas den inre lagrade proceduren eller kod anropet som kapslas.
Ett exempel på en kapslad procedur visas i följande kod:

```sql
CREATE PROCEDURE clean_up @name SYSNAME
AS BEGIN
    EXEC drop_external_table_if_exists @name;
    EXEC drop_external_file_format_if_exists @name;
    EXEC drop_external_data_source_if_exists @name;
END
```

Den här proceduren accepterar en parameter som representerar ett namn och anropar sedan andra procedurer för att släppa objekten med det här namnet.
Synapse SQL-pool stöder högst åtta kapslings nivåer. Den här funktionen skiljer sig något från SQL Server. Kapslings nivån i SQL Server är 32.

Det översta lagrade procedur anropet motsvarar kapslad nivå 1.

```sql
EXEC clean_up 'mytest'
```

Om den lagrade proceduren också gör ett annat EXEC-anrop, ökar kapslings nivån till två.

```sql
CREATE PROCEDURE clean_up @name SYSNAME
AS
    EXEC drop_external_table_if_exists @name  -- This call is nest level 2
GO
EXEC clean_up 'mytest'  -- This call is nest level 1
```

Om den andra proceduren sedan kör en del dynamisk SQL, ökar kapslings nivån till tre.

```sql
CREATE PROCEDURE drop_external_table_if_exists @name SYSNAME
AS BEGIN
    /* See full code in the previous example */
    EXEC sp_executesql @tsql = @drop_stmt;  -- This call is nest level 3
END
GO
CREATE PROCEDURE clean_up @name SYSNAME
AS
    EXEC drop_external_table_if_exists @name  -- This call is nest level 2
GO
EXEC clean_up 'mytest'  -- This call is nest level 1
```

> [!NOTE]
> Synapse SQL stöder för närvarande inte [@ @NESTLEVEL ](/sql/t-sql/functions/nestlevel-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest&preserve-view=true). Du måste spåra kapslings nivån. Det är inte troligt att du överskrider gränsen på åtta kapslings nivåer, men om du gör det måste du återanvända koden för att passa de kapslade nivåerna inom den här gränsen.

## <a name="insertexecute"></a>INSERT..EXESÖTA

Synapse SQL tillåter inte att du använder resultat uppsättningen för en lagrad procedur med en INSERT-instruktion. Det finns en alternativ metod som du kan använda. Ett exempel finns i artikeln om [temporära tabeller](develop-tables-temporary.md).

## <a name="limitations"></a>Begränsningar

Det finns vissa aspekter av lagrade Transact-SQL-procedurer som inte implementeras i Synapse SQL, till exempel:

* temporärt lagrade procedurer
* numrerade lagrade procedurer
* utökade lagrade procedurer
* Lagrade CLR-procedurer
* krypterings alternativ
* replikeringsalternativ
* tabell värdes parametrar
* skrivskyddade parametrar
* standard parametrar (i den etablerade poolen)
* körnings kontexter
* Return-instruktion

## <a name="next-steps"></a>Nästa steg

Mer utvecklings tips finns i [utvecklings översikt](develop-overview.md).
