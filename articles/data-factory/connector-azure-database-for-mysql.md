---
title: Kopiera data till och från Azure Database for MySQL
description: Lär dig hur du kopierar data till och från Azure Database for MySQL med hjälp av en kopierings aktivitet i en Azure Data Factory pipeline.
services: data-factory
ms.author: jingwang
author: linda33wj
manager: shwang
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 08/25/2019
ms.openlocfilehash: bbb4aed8ca10fcf7c15e7442ee7067b2e3f8087d
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/02/2020
ms.locfileid: "81410709"
---
# <a name="copy-data-to-and-from-azure-database-for-mysql-using-azure-data-factory"></a>Kopiera data till och från Azure Database for MySQL med Azure Data Factory

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Den här artikeln beskriver hur du använder kopierings aktiviteten i Azure Data Factory för att kopiera data från Azure Database for MySQL. Den bygger på [översikts artikeln om kopierings aktiviteten](copy-activity-overview.md) som visar en översikt över kopierings aktiviteten.

Den här anslutningen är specialiserad för [Azure Database for MySQL-tjänsten](../mysql/overview.md). Använd [MySQL Connector](connector-mysql.md)om du vill kopiera data från en allmän MySQL-databas som finns lokalt eller i molnet.

## <a name="supported-capabilities"></a>Funktioner som stöds

Den här Azure Database for MySQL anslutningen stöds för följande aktiviteter:

- [Kopierings aktivitet](copy-activity-overview.md) med [matrisen source/Sink som stöds](copy-activity-overview.md)
- [Söknings aktivitet](control-flow-lookup-activity.md)

Du kan kopiera data från Azure Database for MySQL till alla mottagar data lager som stöds. Du kan också kopiera data från alla käll data lager som stöds till Azure Database for MySQL. En lista över data lager som stöds som källor/mottagare av kopierings aktiviteten finns i tabellen över [data lager som stöds](copy-activity-overview.md#supported-data-stores-and-formats) .

Azure Data Factory innehåller en inbyggd driv rutin som möjliggör anslutning, och du behöver därför inte installera någon driv rutin manuellt med hjälp av den här anslutningen.

## <a name="getting-started"></a>Komma igång

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

I följande avsnitt finns information om egenskaper som används för att definiera Data Factory entiteter som är speciella för Azure Database for MySQL koppling.

## <a name="linked-service-properties"></a>Egenskaper för länkad tjänst

Följande egenskaper stöds för Azure Database for MySQL länkade tjänsten:

| Egenskap | Beskrivning | Obligatorisk |
|:--- |:--- |:--- |
| typ | Egenskapen Type måste anges till: **AzureMySql** | Ja |
| Begär | Ange information som krävs för att ansluta till Azure Database for MySQL-instansen. <br/> Du kan också ställa in lösen ord i Azure Key Vault och hämta `password` konfigurationen från anslutnings strängen. Se följande exempel och [lagra autentiseringsuppgifter i Azure Key Vault](store-credentials-in-key-vault.md) artikel med mer information. | Ja |
| connectVia | Den [integration runtime](concepts-integration-runtime.md) som ska användas för att ansluta till data lagret. Du kan använda Azure Integration Runtime eller egen värd Integration Runtime (om ditt data lager finns i privat nätverk). Om inget värde anges används standard Azure Integration Runtime. |Nej |

En typisk anslutnings sträng är `Server=<server>.mysql.database.azure.com;Port=<port>;Database=<database>;UID=<username>;PWD=<password>` . Fler egenskaper som du kan ställa in per ärende:

| Egenskap | Beskrivning | Alternativ | Obligatorisk |
|:--- |:--- |:--- |:--- |
| SSLMode | Det här alternativet anger om driv rutinen använder TLS-kryptering och verifiering vid anslutning till MySQL. T.ex. `SSLMode=<0/1/2/3/4>`| Inaktive rad (0)/PRIORITERAt (1) **(standard)** /obligatoriskt (2)/VERIFY_CA (3)/VERIFY_IDENTITY (4) | Nej |
| UseSystemTrustStore | Det här alternativet anger om du vill använda ett CA-certifikat från arkivet för system förtroende eller från en angiven PEM-fil. T.ex. `UseSystemTrustStore=<0/1>;`| Aktiverat (1)/inaktiverat (0) **(standard)** | Nej |

**Exempel:**

```json
{
    "name": "AzureDatabaseForMySQLLinkedService",
    "properties": {
        "type": "AzureMySql",
        "typeProperties": {
            "connectionString": "Server=<server>.mysql.database.azure.com;Port=<port>;Database=<database>;UID=<username>;PWD=<password>"
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

**Exempel: lagra lösen ord i Azure Key Vault**

```json
{
    "name": "AzureDatabaseForMySQLLinkedService",
    "properties": {
        "type": "AzureMySql",
        "typeProperties": {
            "connectionString": "Server=<server>.mysql.database.azure.com;Port=<port>;Database=<database>;UID=<username>;",
            "password": { 
                "type": "AzureKeyVaultSecret", 
                "store": { 
                    "referenceName": "<Azure Key Vault linked service name>", 
                    "type": "LinkedServiceReference" 
                }, 
                "secretName": "<secretName>" 
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

## <a name="dataset-properties"></a>Egenskaper för datamängd

En fullständig lista över avsnitt och egenskaper som är tillgängliga för att definiera data uppsättningar finns i artikeln [data uppsättningar](concepts-datasets-linked-services.md) . Det här avsnittet innehåller en lista över egenskaper som stöds av Azure Database for MySQL data uppsättning.

Om du vill kopiera data från Azure Database for MySQL anger du egenskapen type för data uppsättningen till **AzureMySqlTable**. Följande egenskaper stöds:

| Egenskap | Beskrivning | Obligatorisk |
|:--- |:--- |:--- |
| typ | Data uppsättningens typ-egenskap måste anges till: **AzureMySqlTable** | Ja |
| tableName | Namnet på tabellen i MySQL-databasen. | Nej (om "fråga" i aktivitets källan har angetts) |

**Exempel**

```json
{
    "name": "AzureMySQLDataset",
    "properties": {
        "type": "AzureMySqlTable",
        "linkedServiceName": {
            "referenceName": "<Azure MySQL linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "tableName": "<table name>"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Kopiera egenskaper för aktivitet

En fullständig lista över avsnitt och egenskaper som är tillgängliga för att definiera aktiviteter finns i artikeln om [pipeliner](concepts-pipelines-activities.md) . Det här avsnittet innehåller en lista över egenskaper som stöds av Azure Database for MySQL källa och mottagare.

### <a name="azure-database-for-mysql-as-source"></a>Azure Database for MySQL som källa

För att kopiera data från Azure Database for MySQL, stöds följande egenskaper i avsnittet Kopiera aktivitets **källa** :

| Egenskap | Beskrivning | Obligatorisk |
|:--- |:--- |:--- |
| typ | Typ egenskapen för kopierings aktivitets källan måste anges till: **AzureMySqlSource** | Ja |
| DocumentDB | Använd den anpassade SQL-frågan för att läsa data. Exempel: `"SELECT * FROM MyTable"`. | Nej (om "tableName" i data uppsättningen har angetts) |
| queryCommandTimeout | Vänte tiden innan fråge förfrågningen nådde tids gränsen. Standardvärdet är 120 minuter (02:00:00) | Nej |

**Exempel:**

```json
"activities":[
    {
        "name": "CopyFromAzureDatabaseForMySQL",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Azure MySQL input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "AzureMySqlSource",
                "query": "<custom query e.g. SELECT * FROM MyTable>"
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

### <a name="azure-database-for-mysql-as-sink"></a>Azure Database for MySQL som mottagare

För att kopiera data till Azure Database for MySQL, stöds följande egenskaper i avsnittet Kopiera aktivitets **mottagare** :

| Egenskap | Beskrivning | Obligatorisk |
|:--- |:--- |:--- |
| typ | Egenskapen Type för kopierings aktivitetens Sink måste anges till: **AzureMySqlSink** | Ja |
| preCopyScript | Ange en SQL-fråga för kopierings aktiviteten som ska köras innan data skrivs till Azure Database for MySQL i varje körning. Du kan använda den här egenskapen för att rensa de förinstallerade data. | Nej |
| writeBatchSize | Infogar data i Azure Database for MySQL tabellen när buffertstorleken når writeBatchSize.<br>Tillåtet värde är Integer som representerar antalet rader. | Nej (standard är 10 000) |
| writeBatchTimeout | Vänte tid för att infoga batch-åtgärden ska slutföras innan tids gränsen uppnåddes.<br>Tillåtna värden är TimeSpan. Ett exempel är 00:30:00 (30 minuter). | Nej (standard är 00:00:30) |

**Exempel:**

```json
"activities":[
    {
        "name": "CopyToAzureDatabaseForMySQL",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<Azure MySQL output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "<source type>"
            },
            "sink": {
                "type": "AzureMySqlSink",
                "preCopyScript": "<custom SQL script>",
                "writeBatchSize": 100000
            }
        }
    }
]
```

## <a name="lookup-activity-properties"></a>Egenskaper för Sök aktivitet

Om du vill veta mer om egenskaperna kontrollerar du [söknings aktiviteten](control-flow-lookup-activity.md).

## <a name="data-type-mapping-for-azure-database-for-mysql"></a>Data typs mappning för Azure Database for MySQL

När du kopierar data från Azure Database for MySQL används följande mappningar från MySQL-datatyper för att Azure Data Factory tillfälliga data typer. Se [mappningar av schema och data typer](copy-activity-schema-and-type-mapping.md) för att lära dig mer om hur kopierings aktiviteten mappar käll schema och datatyp till mottagaren.

| Azure Database for MySQL data typ | Data fabrikens interimistiska datatyp |
|:--- |:--- |
| `bigint` |`Int64` |
| `bigint unsigned` |`Decimal` |
| `bit` |`Boolean` |
| `bit(M), M>1`|`Byte[]`|
| `blob` |`Byte[]` |
| `bool` |`Int16` |
| `char` |`String` |
| `date` |`Datetime` |
| `datetime` |`Datetime` |
| `decimal` |`Decimal, String` |
| `double` |`Double` |
| `double precision` |`Double` |
| `enum` |`String` |
| `float` |`Single` |
| `int` |`Int32` |
| `int unsigned` |`Int64`|
| `integer` |`Int32` |
| `integer unsigned` |`Int64` |
| `long varbinary` |`Byte[]` |
| `long varchar` |`String` |
| `longblob` |`Byte[]` |
| `longtext` |`String` |
| `mediumblob` |`Byte[]` |
| `mediumint` |`Int32` |
| `mediumint unsigned` |`Int64` |
| `mediumtext` |`String` |
| `numeric` |`Decimal` |
| `real` |`Double` |
| `set` |`String` |
| `smallint` |`Int16` |
| `smallint unsigned` |`Int32` |
| `text` |`String` |
| `time` |`TimeSpan` |
| `timestamp` |`Datetime` |
| `tinyblob` |`Byte[]` |
| `tinyint` |`Int16` |
| `tinyint unsigned` |`Int16` |
| `tinytext` |`String` |
| `varchar` |`String` |
| `year` |`Int32` |

## <a name="next-steps"></a>Nästa steg
En lista över data lager som stöds som källor och mottagare av kopierings aktiviteten i Azure Data Factory finns i [data lager som stöds](copy-activity-overview.md#supported-data-stores-and-formats).
