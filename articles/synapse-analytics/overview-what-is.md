---
title: Vad är Azure Synapse Analytics?
description: En översikt över Azure Synapse Analytics
services: synapse-analytics
author: saveenr
ms.service: synapse-analytics
ms.topic: overview
ms.subservice: overview
ms.date: 09/12/2020
ms.author: saveenr
ms.reviewer: jrasnick
ms.openlocfilehash: c4338152579170bf809577262992f0db9a1a95ff
ms.sourcegitcommit: 03662d76a816e98cfc85462cbe9705f6890ed638
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 09/15/2020
ms.locfileid: "90524954"
---
# <a name="what-is-azure-synapse-analytics-workspaces-preview"></a>Vad är Azure Synapse Analytics (för hands versioner av arbets ytor)?

[!INCLUDE [preview](includes/note-preview.md)]

Enterprise Analytics måste arbeta massivt på alla typer av data, oavsett om de är RAW, raffinerade eller mycket anpassade. Detta krävs för att företag ska kunna häfta samman Big data och data lagrings tekniker som Spark och SQL till omfattande datapipeliner som fungerar mellan data i Relations lager och data sjöar. Lösningar som detta är svåra att bygga, säkra och underhålla. Komplexitets fördröjningen för att leverera Insight Enteprises-behovet.

**Azure Synapse** är en integrerad analys tjänst som påskyndar tiden för att bli inblick i informations lager och Big data analys system. Azure-Synapse kombinerar i sin kärna det bästa av **SQL** -teknikerna som används i företags data lager, **Spark** -tekniker som används för Big data och **pipelines** för data integrering och ETL/ELT. Synapse har en webbaserad **Studio** som ger en enda plats för hantering, övervakning, kodning och säkerhet. Synapse innehåller djupgående integrering med andra Azure-tjänster som **PowerBI**, **CosmosDB**och **azureml**.

## <a name="key-features--benefits"></a>Viktiga funktioner & förmåner

### <a name="industry-leading-sql"></a>Branschledande SQL

* **SYNAPSE SQL** är ett distribuerat Query-system som gör det möjligt för företag att implementera data lager-och data virtualiseringslösningar med hjälp av vanliga T-SQL-upplevelser som är bekanta med data tekniker. Den utökar också funktionerna i SQL för att adressera strömnings-och maskin inlärnings scenarier.

* Synapse SQL erbjuder både **Server** lös och **dedikerade** resurs modeller, och erbjuder användnings-och fakturerings alternativ som passar dina behov. För förutsägbara prestanda och kostnader skapar du dedikerade SQL-pooler för att reservera bearbetnings kraften för data som lagras i SQL-tabeller. För oplanerade eller burst-arbetsbelastningar använder du den alltid tillgängliga, Server lös SQL-slutpunkten.
* Använd inbyggda **strömnings** funktioner för att landa data från moln data källor till SQL-tabeller
* Integrera AI med SQL genom att använda **maskin inlärnings** modeller för att räkna data med hjälp av [funktionen T-SQL predict](https://docs.microsoft.com/sql/t-sql/queries/predict-transact-sql?view=azure-sqldw-latest)

### <a name="industry-standard-apache-spark"></a>Bransch standard Apache Spark

**Apache Spark för Azure Synapse** djup och sömlöst integrera Apache Spark – den mest populära Big data motorn med öppen källkod som används för förberedelse av data, data teknik, ETL och maskin inlärning.

* ML-modeller med SparkML-algoritmer och AzureML-integrering för Apache Spark 2,4 med inbyggt stöd för Linux Foundation delta Lake.
* Förenklad resurs modell som gör att du inte behöver oroa dig för att hantera kluster.
* Snabb start av Spark och aggressiv automatisk skalning.
* Inbyggt stöd för .NET för Spark gör att du kan återanvända din C#-expertis och befintliga .NET-kod i ett Spark-program.

### <a name="interop-of-sql-and-apache-spark-on-your-data-lake"></a>Interop för SQL och Apache Spark på din Data Lake

Azure Synapse tar bort de traditionella teknik barriärerna mellan att använda SQL och Spark tillsammans. Du kan kombinera och matcha på ett smidigt sätt utifrån dina behov och kunskaper.

* Ett delat Hive-kompatibelt metadata system gör det möjligt för tabeller som definierats på filer i data Lake att vara sömlöst förbrukade antingen av Spark eller Hive.
* SQL och Spark kan direkt utforska och analysera Parquet-, CSV-, TSV-och JSON-filer som lagras i data Lake.
* Snabb skalbar belastning och borttagning av data som går mellan SQL-och Spark-databaser

### <a name="built-in-data-integration-via-pipelines"></a>Inbyggd data integrering via pipelines

Azure Synapse levereras med samma data integrerings motor och upplevelser som Azure Data Factory, så att du kan skapa omfattande ETL-pipelines utan att lämna Synapse Analytics.

* Mata in data från 90 + data källor
* Kod fri ETL med data flödes aktiviteter
* Dirigera antecknings böcker, Spark-jobb, lagrade procedurer, SQL-skript med mera

### <a name="unified-management-monitoring-and-security"></a>Enhetlig hantering, övervakning och säkerhet

Azure Synapse erbjuder ett enda sätt för företag att hantera analys resurser, övervaka användning och aktivitet och upprätthålla säkerhet.

* Tilldela användare till roll för att förenkla åtkomst till analys resurser
* Detaljerad åtkomst kontroll för data och kod
* En enda instrument panel för att övervaka resurser, användning och användare i SQL och Spark

### <a name="synapse-studio"></a>Synapse Studio

**Synapse Studio** är den webb inbyggda upplevelsen som kopplar ihop allt för data tekniker, så att de kan utföra alla uppgifter som behövs för att skapa en komplett lösning.

* Bygg en lösning från slut punkt till slut punkt på en plats: mata in, utforska, förbereda, dirigera, visualisera
* Branschledande produktivitet för data tekniker som skriver SQL-eller Spark-kod: redigering, fel sökning och prestanda optimering
* Integrera med Enterprise CI/CD-processer

## <a name="next-steps"></a>Nästa steg

* [Kom igång med Azure Synapse Analytics](get-started.md)
* [Skapa en arbetsyta](quickstart-create-workspace.md)
* [Använda SQL på begäran](quickstart-sql-on-demand.md)
