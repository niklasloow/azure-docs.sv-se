---
title: Läsa in data till Azure Synapse Analytics
description: Använd Azure Data Factory för att kopiera data till Azure Synapse Analytics
services: data-factory
ms.author: jingwang
author: linda33wj
manager: shwang
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 06/08/2020
ms.openlocfilehash: c90590ac4c47f6ac8d51273fecfb653dfe056b1d
ms.sourcegitcommit: de2750163a601aae0c28506ba32be067e0068c0c
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 09/04/2020
ms.locfileid: "89485778"
---
# <a name="load-data-into-azure-synapse-analytics-by-using-azure-data-factory"></a>Läs in data i Azure Synapse Analytics med hjälp av Azure Data Factory

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

[Azure Synapse Analytics](../synapse-analytics/sql-data-warehouse/sql-data-warehouse-overview-what-is.md) är en molnbaserad, skalbar databas som kan bearbeta stora mängder data, både relationella och icke-relationella. Azure Synapse Analytics bygger på den minnes trycks arkitektur (massivt parallell bearbetning) som är optimerad för arbets belastningar för företags data lager. Det ger moln elastiskhet med flexibiliteten att skala lagring och beräkning oberoende av varandra.

Att komma igång med Azure Synapse Analytics är nu enklare än någonsin när du använder Azure Data Factory. Azure Data Factory är en helt hanterad molnbaserad data integrerings tjänst. Du kan använda tjänsten för att fylla i en Azure Synapse-analys med data från ditt befintliga system och spara tid när du skapar dina analys lösningar.

Azure Data Factory erbjuder följande fördelar för att läsa in data i Azure Synapse Analytics:

* **Enkelt att konfigurera**: en intuitiv 5-stegs guide utan skript krävs.
* **Stöd för omfattande data lager**: inbyggt stöd för en omfattande uppsättning lokala och molnbaserade data lager. En detaljerad lista finns i tabellen över [data lager som stöds](copy-activity-overview.md#supported-data-stores-and-formats).
* **Säkert och kompatibelt**: data överförs via https eller ExpressRoute. Med den globala tjänstens närvaro ser du till att dina data aldrig lämnar den geografiska gräns.
* **Oöverträffade prestanda med hjälp av PolyBase**: Polybase är det mest effektiva sättet att flytta data till Azure Synapse Analytics. Använd funktionen för mellanlagring av BLOB för att uppnå höga belastnings hastigheter från alla typer av data lager, inklusive Azure Blob Storage och Data Lake Store. (PolyBase stöder Azure Blob Storage och Azure Data Lake Store som standard.) Mer information finns i [Kopiera aktivitets prestanda](copy-activity-performance.md).

Den här artikeln visar hur du använder verktyget Data Factory Kopiera data för att _läsa in data från Azure SQL Database till Azure Synapse Analytics_. Du kan följa liknande steg för att kopiera data från andra typer av data lager.

> [!NOTE]
> Mer information finns i [Kopiera data till eller från Azure Synapse Analytics med hjälp av Azure Data Factory](connector-azure-sql-data-warehouse.md).

## <a name="prerequisites"></a>Krav

* Azure-prenumeration: om du inte har en Azure-prenumeration kan du skapa ett [kostnads fritt konto](https://azure.microsoft.com/free/) innan du börjar.
* Azure Synapse Analytics: data lagret innehåller de data som kopieras från SQL-databasen. Om du inte har en Azure Synapse-analys kan du läsa anvisningarna i [skapa en Azure Synapse-analys](../sql-data-warehouse/sql-data-warehouse-get-started-tutorial.md).
* Azure SQL Database: den här självstudien kopierar data från Adventure Works LT exempel data uppsättning i Azure SQL Database. Du kan skapa den här exempel databasen i SQL Database genom att följa anvisningarna i [skapa en exempel databas i Azure SQL Database](../azure-sql/database/single-database-create-quickstart.md).
* Azure Storage-konto: Azure Storage används som _mellanlagrings_ -BLOB i Mass kopierings åtgärden. Om du inte har något Azure-lagringskonto finns det anvisningar i [Skapa ett lagringskonto](../storage/common/storage-account-create.md).

## <a name="create-a-data-factory"></a>Skapa en datafabrik

1. På den vänstra menyn väljer du **skapa en resurs**  >  **data och analys**  >  **Data Factory**:

2. På sidan **ny data fabrik** anger du värden för följande objekt:

    * **Namn**: ange *LoadSQLDWDemo* som namn. Namnet på data fabriken måste vara * globalt unikt. Om du får felet "data fabriks namnet" LoadSQLDWDemo "är inte tillgängligt" anger du ett annat namn på data fabriken. Du kan till exempel använda namnet _**dittnamn**_**ADFTutorialDataFactory**. Försök att skapa data fabriken igen. Se artikeln [Data Factory – namnregler](naming-rules.md) för namnregler för Data Factory-artefakter.
    * **Prenumeration**: Välj din Azure-prenumeration där du vill skapa data fabriken. 
    * **Resurs grupp**: Välj en befintlig resurs grupp i list rutan eller Välj alternativet för att **Skapa nytt** och ange namnet på en resurs grupp. Mer information om resursgrupper finns i [Använda resursgrupper till att hantera Azure-resurser](../azure-resource-manager/management/overview.md).  
    * **Version**: Välj **V2**.
    * **Plats**: Välj plats för data fabriken. Endast platser som stöds visas i listrutan. De data lager som används av Data Factory kan finnas på andra platser och regioner. Dessa data lager omfattar Azure Data Lake Store, Azure Storage, Azure SQL Database och så vidare.

3. Välj **Skapa**.
4. När du har skapat filen går du till din data fabrik. Du ser **Data Factory** start sida så som visas i följande bild:

   ![Datafabrikens startsida](./media/doc-common-process/data-factory-home-page.png)

   Välj panelen **Författare och övervakare** för att starta dataintegreringsprogrammet på en separat flik.

## <a name="load-data-into-azure-synapse-analytics"></a>Läsa in data till Azure Synapse Analytics

1. På sidan **Kom igång** väljer du panelen **Kopiera data** för att starta verktyget Kopiera data.

1. På sidan **Egenskaper** anger du **CopyFromSQLToSQLDW** för fältet **uppgifts namn** och väljer **Nästa**.

    ![Sidan Egenskaper](./media/load-azure-sql-data-warehouse/copy-data-tool-properties-page.png)

1. Utför följande steg på sidan **käll data lager** :
    >[!TIP]
    >I den här självstudien använder du *SQL-autentisering* som autentiseringstyp för ditt käll data lager, men du kan välja andra autentiseringsmetoder som stöds:*tjänstens huvud namn* och *hanterad identitet* om det behövs. Mer information finns i motsvarande avsnitt i [den här artikeln](https://docs.microsoft.com/azure/data-factory/connector-azure-sql-database#linked-service-properties) .
    >För att lagra hemligheter för data lager säkert rekommenderar vi också att du använder en Azure Key Vault. Se [den här artikeln](https://docs.microsoft.com/azure/data-factory/store-credentials-in-key-vault) för detaljerade illustrationer.

    a. Klicka på **+ Skapa ny anslutning**.

    b. Välj **Azure SQL Database** i galleriet och välj **Fortsätt**. Du kan skriva "SQL" i sökrutan för att filtrera kopplingarna.

    ![Välj Azure SQL DB](./media/load-azure-sql-data-warehouse/select-azure-sql-db-source.png)

    c. På sidan **ny länkad tjänst** väljer du Server namnet och databas namnet i list rutan och anger användar namn och lösen ord. Verifiera inställningarna genom att klicka på **Testa anslutning** och välj sedan **skapa**.

    ![Konfigurera Azure SQL DB](./media/load-azure-sql-data-warehouse/configure-azure-sql-db.png)

    d. Välj den nyligen skapade länkade tjänsten som källa och klicka sedan på **Nästa**.

1. I listan **Välj tabeller som data ska kopieras från eller Använd en anpassad fråga** anger du **SalesLT** för att filtrera tabellerna. Välj rutan **(Markera alla)** om du vill använda alla tabeller för kopian och välj sedan **Nästa**.

    ![Välj käll tabeller](./media/load-azure-sql-data-warehouse/select-source-tables.png)

1. På sidan **Använd filter** anger du inställningarna eller väljer **Nästa**.

1. Utför följande steg på sidan **mål data lager** :
    >[!TIP]
    >I den här självstudien använder du *SQL-autentisering* som autentiseringstyp för mål data lagret, men du kan välja andra autentiseringsmetoder som stöds:*tjänstens huvud namn* och *hanterad identitet* om det behövs. Mer information finns i motsvarande avsnitt i [den här artikeln](https://docs.microsoft.com/azure/data-factory/connector-azure-sql-data-warehouse#linked-service-properties) .
    >För att lagra hemligheter för data lager säkert rekommenderar vi också att du använder en Azure Key Vault. Se [den här artikeln](https://docs.microsoft.com/azure/data-factory/store-credentials-in-key-vault) för detaljerade illustrationer.

    a. Klicka på **+ Skapa ny anslutning** för att lägga till en anslutning

    b. Välj **Azure Synapse Analytics (tidigare SQL Data Warehouse)** från galleriet och välj **Fortsätt**. Du kan skriva "SQL" i sökrutan för att filtrera kopplingarna.

    ![Välj Azure Synapse Analytics](./media/load-azure-sql-data-warehouse/select-azure-sql-dw-sink.png)

    c. På sidan **ny länkad tjänst** väljer du Server namnet och databas namnet i list rutan och anger användar namn och lösen ord. Verifiera inställningarna genom att klicka på **Testa anslutning** och välj sedan **skapa**.

    ![Konfigurera Azure Synapse Analytics](./media/load-azure-sql-data-warehouse/configure-azure-sql-dw.png)

    d. Välj den nyligen skapade länkade tjänsten som mottagare och klicka sedan på **Nästa**.

1. På sidan **tabell mappning** granskar du innehållet och väljer **Nästa**. En intelligent tabell mappning visas. Käll tabellerna mappas till mål tabellerna baserat på tabell namn. Om en käll tabell inte finns i målet skapar Azure Data Factory en mål tabell med samma namn som standard. Du kan också mappa en käll tabell till en befintlig mål tabell.

   > [!NOTE]
   > Automatisk tabell skapande för Azure Synapse Analytics-mottagaren gäller när SQL Server eller Azure SQL Database är källan. Om du kopierar data från ett annat käll data lager måste du skapa schemat i Azure Synapse Analytics innan du kör data kopieringen.

   ![Sidan för tabellmappning](./media/load-azure-sql-data-warehouse/table-mapping.png)

1. Granska innehållet på sidan **kolumn mappning** och välj **Nästa**. Den intelligenta tabell mappningen baseras på kolumn namnet. Om du låter Data Factory skapa tabeller automatiskt, kan data typ konverteringen ske när det finns inkompatibiliteter mellan käll-och mål arkiven. Om det finns en data typs konvertering som inte stöds mellan kolumnen källa och mål visas ett fel meddelande bredvid motsvarande tabell.

    ![Kolumn mappnings sida](./media/load-azure-sql-data-warehouse/schema-mapping.png)

1. Utför följande steg på sidan **Inställningar** :

    a. I avsnittet **mellanlagrings inställningar** klickar du på **+ ny** för att välja ny mellanlagring av mellanlagring. Lagrings utrymmet används för att mellanlagra data innan de läses in i Azure Synapse Analytics genom att använda PolyBase. När kopieringen är klar rensas tillfälliga data i Azure Blob Storage automatiskt.

    b. På sidan **ny länkad tjänst** väljer du ditt lagrings konto och väljer **skapa** för att distribuera den länkade tjänsten.

    c. I avsnittet **Avancerade inställningar** avmarkerar du alternativet **Använd typ standard** och väljer sedan **Nästa**.

    ![Konfigurera PolyBase](./media/load-azure-sql-data-warehouse/configure-polybase.png)

1. På sidan **Sammanfattning** granskar du inställningarna och väljer **Nästa**.

    ![Sammanfattningssida](./media/load-azure-sql-data-warehouse/summary-page.png)

1. Välj **Övervaka** på sidan **Distribution** för att övervaka pipelinen (aktiviteten). 
 
1. Observera att fliken **Övervaka** till vänster väljs automatiskt. När pipeline-körningen har slutförts, väljer du länken **CopyFromSQLToSQLDW** i kolumnen **pipeline-namn** för att Visa körnings information för aktiviteten eller köra pipelinen igen.

    [![Övervaka pipelinekörningar](./media/load-azure-sql-data-warehouse/pipeline-monitoring.png)](./media/load-azure-sql-data-warehouse/pipeline-monitoring.png#lightbox)

1. Om du vill växla tillbaka till vyn pipelines körs väljer du länken **alla pipeline-körningar** överst. Om du vill uppdatera listan väljer du **Refresh** (Uppdatera).

    ![Övervaka aktivitetskörningar](./media/load-azure-sql-data-warehouse/activity-monitoring.png)

1. Om du vill övervaka körnings informationen för varje kopierings aktivitet väljer du länken **information** (glasögon ikonen) under **aktivitets namn** i vyn aktivitets körningar. Du kan övervaka information om data volymen som kopieras från källan till mottagaren, data genom strömning, körnings steg med motsvarande varaktighet och använda konfigurationer.
    ![Övervaka körnings information för aktivitet](./media/load-azure-sql-data-warehouse/monitor-activity-run-details-1.png)

    ![Övervaka körnings information för aktivitet](./media/load-azure-sql-data-warehouse/monitor-activity-run-details-2.png)

## <a name="next-steps"></a>Nästa steg

Gå vidare till följande artikel om du vill lära dig mer om Azure Synapse Analytics-support:

> [!div class="nextstepaction"]
>[Azure Synapse Analytics-anslutning](connector-azure-sql-data-warehouse.md)
