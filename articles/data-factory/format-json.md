---
title: JSON-format i Azure Data Factory
description: I det här avsnittet beskrivs hur du hanterar JSON-format i Azure Data Factory.
author: linda33wj
manager: shwang
ms.reviewer: craigg
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 09/15/2020
ms.author: jingwang
ms.openlocfilehash: 0b80c2f125e7eddaa427b75e3e0fe2aff6c679be
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 09/25/2020
ms.locfileid: "91334335"
---
# <a name="json-format-in-azure-data-factory"></a>JSON-format i Azure Data Factory

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Följ den här artikeln när du vill **parsa JSON-filerna eller skriva data till JSON-format**. 

JSON-format stöds för följande anslutningar: [Amazon S3](connector-amazon-simple-storage-service.md), [Azure Blob](connector-azure-blob-storage.md), [Azure Data Lake Storage Gen1](connector-azure-data-lake-store.md), [Azure Data Lake Storage Gen2](connector-azure-data-lake-storage.md), [Azure File Storage](connector-azure-file-storage.md), [fil system](connector-file-system.md), [FTP](connector-ftp.md), [Google Cloud Storage](connector-google-cloud-storage.md), [HDFS](connector-hdfs.md), [http](connector-http.md)och [SFTP](connector-sftp.md).

## <a name="dataset-properties"></a>Egenskaper för datamängd

En fullständig lista över avsnitt och egenskaper som är tillgängliga för att definiera data uppsättningar finns i artikeln [data uppsättningar](concepts-datasets-linked-services.md) . Det här avsnittet innehåller en lista över egenskaper som stöds av JSON-datauppsättningen.

| Egenskap         | Beskrivning                                                  | Krävs |
| ---------------- | ------------------------------------------------------------ | -------- |
| typ             | Data uppsättningens typ-egenskap måste anges till **JSON**. | Yes      |
| location         | Plats inställningar för filen/filerna. Varje filbaserad koppling har sin egen plats typ och de egenskaper som stöds under `location` . **Se information i avsnittet kopplings artikel – egenskaper för > data uppsättning**. | Yes      |
| encodingName     | Kodnings typen som används för att läsa/skriva testfiler. <br>Tillåtna värden är följande: "UTF-8", "UTF-16", "UTF-16BE", "UTF-32", "UTF-32BE", "US-ASCII", "UTF-7", "BIG5", "EUC-JP", "EUC-KR", "GB2312", "GB18030", "JOHAB", "SHIFT-JIS", "CP875", "CP866", "IBM00858", "IBM037", "IBM273", "IBM437", "IBM500", "IBM737", "IBM775", "IBM850", "IBM852", "IBM855", "IBM861", "IBM863", "IBM864", "IBM865", "IBM869", "IBM870", "IBM01140", "IBM01141", "IBM01142", "IBM01143", "IBM01144", "IBM01145", "IBM01146", "IBM01147", "IBM01148", "", "ISO-2022-JP", "ISO-2022-KR", "ISO-8859-1", "ISO-8859-2", "ISO-8859-3", "ISO-8859-4", "ISO-8859-5", "ISO-8859-6", "ISO-8859-7", "ISO-8859-8", "ISO-8859-9", "ISO-8859-13" , "ISO-8859-15", "WINDOWS-874", "WINDOWS-1250", "WINDOWS-1251", "WINDOWS-1252", "WINDOWS-1253", "WINDOWS-1254", "WINDOWS-1255", "WINDOWS-1256", "WINDOWS-1257", "WINDOWS-1258".| No       |
| komprimering | Grupp egenskaper för att konfigurera fil komprimering. Konfigurera det här avsnittet när du vill utföra komprimering/expandering under aktivitets körningen. | No |
| typ<br/>(*under `compression` *) | Komprimerings-codec som används för att läsa/skriva JSON-filer. <br>Tillåtna värden är **bzip2**, **gzip**, **DEFLATE**, **ZipDeflate**, **TarGzip**, **fästfunktionen**eller **lz4**. Standardvärdet är inte komprimerat.<br>**Obs!** kopierings aktiviteten stöder för närvarande inte "fästfunktionen" & "lz4" och kart data flödet stöder inte "ZipDeflate".<br>**Obs!** när du använder kopierings aktivitet för att expandera **ZipDeflate** / **TarGzip** -fil (er) och skriva till filbaserat mottagar data lager, extraheras filerna som standard till mappen: `<path specified in dataset>/<folder named as source compressed file>/` , används `preserveZipFileNameAsFolder` / `preserveCompressionFileNameAsFolder` vid [kopiering av aktivitets källan](#json-as-source) för att kontrol lera om namnet på den komprimerade filen/filerna är kvar som mappstruktur.| Nej.  |
| nivå<br/>(*under `compression` *) | Komprimerings förhållandet. <br>Tillåtna värden är **optimalt** eller **snabbast**.<br>- **Snabbast:** Komprimerings åtgärden bör utföras så snabbt som möjligt, även om den resulterande filen inte komprimeras optimalt.<br>- **Optimalt**: komprimerings åtgärden bör komprimeras optimalt, även om åtgärden tar längre tid att slutföra. Mer information finns i avsnittet [komprimerings nivå](https://msdn.microsoft.com/library/system.io.compression.compressionlevel.aspx) . | No       |

Nedan visas ett exempel på en JSON-datauppsättning på Azure Blob Storage:

```json
{
    "name": "JSONDataset",
    "properties": {
        "type": "Json",
        "linkedServiceName": {
            "referenceName": "<Azure Blob Storage linked service name>",
            "type": "LinkedServiceReference"
        },
        "schema": [ < physical schema, optional, retrievable during authoring > ],
        "typeProperties": {
            "location": {
                "type": "AzureBlobStorageLocation",
                "container": "containername",
                "folderPath": "folder/subfolder",
            },
            "compression": {
                "type": "gzip"
            }
        }
    }
}
```

## <a name="copy-activity-properties"></a>Kopiera egenskaper för aktivitet

En fullständig lista över avsnitt och egenskaper som är tillgängliga för att definiera aktiviteter finns i artikeln om [pipeliner](concepts-pipelines-activities.md) . Det här avsnittet innehåller en lista över egenskaper som stöds av JSON-källan och mottagare.

Lär dig mer om hur du extraherar data från JSON-filer och mappar till Sink data Store/format eller vice versa från [schema mappning](copy-activity-schema-and-type-mapping.md).

### <a name="json-as-source"></a>JSON som källa

Följande egenskaper stöds i avsnittet Kopiera aktivitets *** \* källa \* *** .

| Egenskap      | Beskrivning                                                  | Krävs |
| ------------- | ------------------------------------------------------------ | -------- |
| typ          | Typ egenskapen för kopierings aktivitets källan måste anges till **JSONSource**. | Yes      |
| formatSettings | En grupp med egenskaper. Se tabellen **JSON Read Settings** nedan. | No       |
| storeSettings | En grupp egenskaper för att läsa data från ett data lager. Varje filbaserad koppling har sina egna Läs inställningar som stöds under `storeSettings` . **Se information i kopplings artikeln – > avsnittet Egenskaper för kopierings aktivitet**. | No       |

**JSON-läsa inställningar** som stöds under `formatSettings` :

| Egenskap      | Beskrivning                                                  | Krävs |
| ------------- | ------------------------------------------------------------ | -------- |
| typ          | Typen för formatSettings måste anges till **JsonReadSettings**. | Yes      |
| compressionProperties | En grupp egenskaper för hur man dekomprimerar data för en angiven komprimerings-codec. | No       |
| preserveZipFileNameAsFolder<br>(*under `compressionProperties` -> `type` som `ZipDeflateReadSettings` *)  | Gäller när indata-dataset konfigureras med **ZipDeflate** -komprimering. Anger om käll filens zip-filnamn ska bevaras som mappstruktur under kopieringen.<br>-Om värdet är **true (standard)**, Data Factory skriver zippade filer till `<path specified in dataset>/<folder named as source zip file>/` .<br>– Om värdet är **false**skriver data Factory zippade filer direkt till `<path specified in dataset>` . Se till att du inte har dubbla fil namn i olika käll-zip-filer för att undvika racing eller oväntat beteende.  | No |
| preserveCompressionFileNameAsFolder<br>(*under `compressionProperties` -> `type` som `TarGZipReadSettings` *) | Gäller när indata-dataset konfigureras med **TarGzip** -komprimering. Anger om du vill bevara det komprimerade fil namnet för källan som mappstruktur under kopieringen.<br>– När värdet är **true (standard)**, Data Factory skriver expanderade filer till `<path specified in dataset>/<folder named as source compressed file>/` . <br>– Om det är inställt på **false**Data Factory skriver expanderade filer direkt till `<path specified in dataset>` . Se till att du inte har dubbla fil namn i olika källfiler för att undvika racing eller oväntat beteende. | No |

### <a name="json-as-sink"></a>JSON som mottagare

Följande egenskaper stöds i avsnittet Kopiera aktivitets *** \* mottagare \* *** .

| Egenskap      | Beskrivning                                                  | Krävs |
| ------------- | ------------------------------------------------------------ | -------- |
| typ          | Typ egenskapen för kopierings aktivitets källan måste anges till **JSONSink**. | Yes      |
| formatSettings | En grupp med egenskaper. Se tabellen **JSON Write Settings** nedan. | No       |
| storeSettings | En grupp egenskaper för hur du skriver data till ett data lager. Varje filbaserad koppling har sina egna Skriv inställningar som stöds under `storeSettings` . **Se information i kopplings artikeln – > avsnittet Egenskaper för kopierings aktivitet**. | No       |

**JSON-Skriv inställningar** som stöds under `formatSettings` :

| Egenskap      | Beskrivning                                                  | Krävs                                              |
| ------------- | ------------------------------------------------------------ | ----------------------------------------------------- |
| typ          | Typen för formatSettings måste anges till **JsonWriteSettings**. | Yes                                                   |
| filePattern |Ange mönstret för de data som lagras i varje JSON-fil. Tillåtna värden är: **setOfObjects** (JSON-linjer) och **arrayOfObjects**. **Standardvärdet** är **setOfObjects**. Detaljerad information om dessa mönster finns i avsnittet om [JSON-filmönster](#json-file-patterns). |No |

### <a name="json-file-patterns"></a>JSON-filmönster

När du kopierar data från JSON-filer kan kopierings aktiviteten automatiskt identifiera och analysera följande mönster i JSON-filer. När du skriver data till JSON-filer kan du konfigurera fil mönstret för kopierings aktivitets mottagare.

- **Typ I: setOfObjects**

    Varje fil innehåller enstaka objekt, JSON-linjer eller sammanfogade objekt.

    * **Exempel på JSON med enskilda objekt**

        ```json
        {
            "time": "2015-04-29T07:12:20.9100000Z",
            "callingimsi": "466920403025604",
            "callingnum1": "678948008",
            "callingnum2": "567834760",
            "switch1": "China",
            "switch2": "Germany"
        }
        ```

    * **JSON-linjer (standard för handfat)**

        ```json
        {"time":"2015-04-29T07:12:20.9100000Z","callingimsi":"466920403025604","callingnum1":"678948008","callingnum2":"567834760","switch1":"China","switch2":"Germany"}
        {"time":"2015-04-29T07:13:21.0220000Z","callingimsi":"466922202613463","callingnum1":"123436380","callingnum2":"789037573","switch1":"US","switch2":"UK"}
        {"time":"2015-04-29T07:13:21.4370000Z","callingimsi":"466923101048691","callingnum1":"678901578","callingnum2":"345626404","switch1":"Germany","switch2":"UK"}
        ```

    * **Exempel med sammanfogad JSON**

        ```json
        {
            "time": "2015-04-29T07:12:20.9100000Z",
            "callingimsi": "466920403025604",
            "callingnum1": "678948008",
            "callingnum2": "567834760",
            "switch1": "China",
            "switch2": "Germany"
        }
        {
            "time": "2015-04-29T07:13:21.0220000Z",
            "callingimsi": "466922202613463",
            "callingnum1": "123436380",
            "callingnum2": "789037573",
            "switch1": "US",
            "switch2": "UK"
        }
        {
            "time": "2015-04-29T07:13:21.4370000Z",
            "callingimsi": "466923101048691",
            "callingnum1": "678901578",
            "callingnum2": "345626404",
            "switch1": "Germany",
            "switch2": "UK"
        }
        ```

- **Typ II: arrayOfObjects**

    Varje fil innehåller en matris med objekt.

    ```json
    [
        {
            "time": "2015-04-29T07:12:20.9100000Z",
            "callingimsi": "466920403025604",
            "callingnum1": "678948008",
            "callingnum2": "567834760",
            "switch1": "China",
            "switch2": "Germany"
        },
        {
            "time": "2015-04-29T07:13:21.0220000Z",
            "callingimsi": "466922202613463",
            "callingnum1": "123436380",
            "callingnum2": "789037573",
            "switch1": "US",
            "switch2": "UK"
        },
        {
            "time": "2015-04-29T07:13:21.4370000Z",
            "callingimsi": "466923101048691",
            "callingnum1": "678901578",
            "callingnum2": "345626404",
            "switch1": "Germany",
            "switch2": "UK"
        }
    ]
    ```

## <a name="mapping-data-flow-properties"></a>Mappa data flödes egenskaper

I mappa data flöden kan du läsa och skriva till JSON-format i följande data lager: [Azure Blob Storage](connector-azure-blob-storage.md#mapping-data-flow-properties), [Azure Data Lake Storage gen1](connector-azure-data-lake-store.md#mapping-data-flow-properties)och [Azure Data Lake Storage Gen2](connector-azure-data-lake-storage.md#mapping-data-flow-properties).

### <a name="source-properties"></a>Käll egenskaper

I tabellen nedan visas de egenskaper som stöds av en JSON-källa. Du kan redigera dessa egenskaper på fliken **käll alternativ** .

| Name | Beskrivning | Krävs | Tillåtna värden | Skript egenskap för data flöde |
| ---- | ----------- | -------- | -------------- | ---------------- |
| Jokertecken sökvägar | Alla filer som matchar sökvägen för jokertecken kommer att bearbetas. Åsidosätter mappen och fil Sök vägen som angetts i data uppsättningen. | nej | Sträng [] | wildcardPaths |
| Partitionens rot Sök väg | För fildata som är partitionerade kan du ange en rot Sök väg för partitionen för att kunna läsa partitionerade mappar som kolumner | nej | Sträng | partitionRootPath |
| Lista över filer | Om källan pekar på en textfil som visar en lista över filer som ska bearbetas | nej | `true` eller `false` | fileList |
| Kolumn som ska lagra fil namn | Skapa en ny kolumn med käll filens namn och sökväg | nej | Sträng | rowUrlColumn |
| Efter slut för ande | Ta bort eller flytta filerna efter bearbetning. Fil Sök vägen börjar från container roten | nej | Ta bort: `true` eller `false` <br> Fart `['<from>', '<to>']` | purgeFiles <br> moveFiles |
| Filtrera efter senast ändrad | Välj att filtrera filer baserat på när de senast ändrades | nej | Timestamp | modifiedAfter <br> modifiedBefore |
| Enstaka dokument | Mappa data flöden läsa ett JSON-dokument från varje fil | nej | `true` eller `false` | singleDocument |
| Icke-citerade kolumn namn | Om du väljer **icke-citerade kolumn namn** , läser mappningen av data flöden JSON-kolumner som inte omges av citat tecken. | nej | `true` eller `false` |  unquotedColumnNames |
| Har kommentarer | Välj **innehåller kommentarer** om JSON-data har C eller C++-format kommentar | nej | `true` eller `false` | Kommentarer |
| Enkelt citat tecken | Läser JSON-kolumner som inte omges av citat tecken | nej | `true` eller `false` | singleQuoted |
| Omvänt snedstreck Escaped | Välj **omvänt snedstreck Escaped** om omvända snedstreck används för att undanta tecken i JSON-data | nej | `true` eller `false` | backslashEscape |
| Det gick inte att hitta några filer | Om värdet är true uppstår ett fel inte om inga filer hittas | nej | `true` eller `false` | ignoreNoFilesFound |

### <a name="source-format-options"></a>Käll format alternativ

Genom att använda en JSON-datauppsättning som källa i ditt data flöde kan du ange fem ytterligare inställningar. De här inställningarna finns under inställningen för **JSON-inställningar** på fliken **käll alternativ** .  

![JSON-inställningar](media/data-flow/json-settings.png "JSON-inställningar")

#### <a name="default"></a>Standardvärde

Som standard läses JSON-data in i följande format.

```
{ "json": "record 1" }
{ "json": "record 2" }
{ "json": "record 3" }
```

#### <a name="single-document"></a>Enstaka dokument

Om **ett enstaka dokument** är markerat, kan mappning av data flöda ett JSON-dokument från varje fil. 

``` json
File1.json
{
    "json": "record 1"
}
File2.json
{
    "json": "record 2"
}
File3.json
{
    "json": "record 3"
}
```
> [!NOTE]
> Om data flöden genererar ett fel som anger "corrupt_record" vid för hands visning av JSON-data, är det troligt att dina data innehåller ett enda dokument i JSON-filen. Inställningen "enstaka dokument" bör ta bort felet.

#### <a name="unquoted-column-names"></a>Icke-citerade kolumn namn

Om du väljer **icke-citerade kolumn namn** , läser mappningen av data flöden JSON-kolumner som inte omges av citat tecken. 

```
{ json: "record 1" }
{ json: "record 2" }
{ json: "record 3" }
```

#### <a name="has-comments"></a>Har kommentarer

Välj **innehåller kommentarer** om JSON-data har C-eller C++-format.

``` json
{ "json": /** comment **/ "record 1" }
{ "json": "record 2" }
{ /** comment **/ "json": "record 3" }
```

#### <a name="single-quoted"></a>Enkelt citat tecken

Välj **enkelt citat tecken** om JSON-fälten och-värdena använder enkla citat tecken i stället för dubbla citat tecken.

```
{ 'json': 'record 1' }
{ 'json': 'record 2' }
{ 'json': 'record 3' }
```

#### <a name="backslash-escaped"></a>Omvänt snedstreck Escaped

Välj **omvänt snedstreck Escaped** om omvända snedstreck används för att undanta tecken i JSON-data.

```
{ "json": "record 1" }
{ "json": "\} \" \' \\ \n \\n record 2" }
{ "json": "record 3" }
```

### <a name="sink-properties"></a>Egenskaper för mottagare

I tabellen nedan visas de egenskaper som stöds av en JSON-mottagare. Du kan redigera dessa egenskaper på fliken **Inställningar** .

| Name | Beskrivning | Krävs | Tillåtna värden | Skript egenskap för data flöde |
| ---- | ----------- | -------- | -------------- | ---------------- |
| Rensa mappen | Om målmappen rensas innan den skrivs | nej | `true` eller `false` | truncate |
| Fil namns alternativ | Namngivnings formatet för de data som skrivits. Som standard är en fil per partition i format `part-#####-tid-<guid>` | nej | Mönster: sträng <br> Per partition: sträng [] <br> Som data i kolumnen: sträng <br> Utdata till en enskild fil: `['<fileName>']`  | filePattern <br> partitionFileNames <br> rowUrlColumn <br> partitionFileNames |

### <a name="creating-json-structures-in-a-derived-column"></a>Skapa JSON-strukturer i en härledd kolumn

Du kan lägga till en komplex kolumn i ditt data flöde via uttrycks verktyget härledd kolumn. I den härledda kolumn omvandlingen lägger du till en ny kolumn och öppnar uttrycks verktyget genom att klicka på den blå rutan. Om du vill göra en kolumn komplex kan du ange JSON-strukturen manuellt eller använda UX för att lägga till under kolumner interaktivt.

#### <a name="using-the-expression-builder-ux"></a>Använda Expression Builder UX

Hovra över en kolumn i fönstret utdata schema och klicka på plus ikonen. Välj **Lägg till under kolumn** för att göra kolumnen till en komplex typ.

![Lägg till under kolumn](media/data-flow/derive-add-subcolumn.png "Lägg till under kolumn")

Du kan lägga till ytterligare kolumner och under kolumner på samma sätt. För varje icke-komplext fält kan ett uttryck läggas till i uttrycks redigeraren till höger.

![Lägg till komplex kolumn](media/data-flow/derive-complex-column.png "Lägg till kolumner")

#### <a name="entering-the-json-structure-manually"></a>Ange JSON-strukturen manuellt

Om du vill lägga till en JSON-struktur manuellt lägger du till en ny kolumn och anger uttrycket i redigeraren. Uttrycket följer följande allmänna format:

```
@(
    field1=0,
    field2=@(
        field1=0
    )
)
```

Om det här uttrycket angavs för en kolumn med namnet "complexColumn", skrivs det till mottagaren som följande JSON:

```
{
    "complexColumn": {
        "field1": 0,
        "field2": {
            "field1": 0
        }
    }
}
```

#### <a name="sample-manual-script-for-complete-hierarchical-definition"></a>Exempel på manuellt skript för fullständig hierarkisk definition
```
@(
    title=Title,
    firstName=FirstName,
    middleName=MiddleName,
    lastName=LastName,
    suffix=Suffix,
    contactDetails=@(
        email=EmailAddress,
        phone=Phone
    ),
    address=@(
        line1=AddressLine1,
        line2=AddressLine2,
        city=City,
        state=StateProvince,
        country=CountryRegion,
        postCode=PostalCode
    ),
    ids=[
        toString(CustomerID), toString(AddressID), rowguid
    ]
)
```

## <a name="next-steps"></a>Nästa steg

- [Översikt över kopieringsaktivitet](copy-activity-overview.md)
- [Mappa data flöde](concepts-data-flow-overview.md)
- [Söknings aktivitet](control-flow-lookup-activity.md)
- [GetMetadata-aktivitet](control-flow-get-metadata-activity.md)
