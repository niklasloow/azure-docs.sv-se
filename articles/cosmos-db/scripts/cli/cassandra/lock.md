---
title: Skapa resurs lås för ett Cassandra-tecken och en tabell för Azure Cosmos DB
description: Skapa resurs lås för ett Cassandra-tecken och en tabell för Azure Cosmos DB
author: markjbrown
ms.author: mjbrown
ms.service: cosmos-db
ms.subservice: cosmosdb-cassandra
ms.topic: sample
ms.date: 07/29/2020
ms.openlocfilehash: bbb6f048f131a38ceaba244e4d8962f97e9c1431
ms.sourcegitcommit: 0b8320ae0d3455344ec8855b5c2d0ab3faa974a3
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/30/2020
ms.locfileid: "87429684"
---
# <a name="create-a-resource-lock-for-azure-cosmos-cassandra-api-keyspace-and-table-using-azure-cli"></a>Skapa ett resurs lås för Azure Cosmos API för Cassandra-tecken utrymme och tabell med Azure CLI

[!INCLUDE [cloud-shell-try-it.md](../../../../../includes/cloud-shell-try-it.md)]

Om du väljer att installera och använda CLI lokalt, kräver det här avsnittet att du kör Azure CLI-version 2.9.1 eller senare. Kör `az --version` för att hitta versionen. Om du behöver installera eller uppgradera kan du läsa [Installera Azure CLI](/cli/azure/install-azure-cli).

> [!IMPORTANT]
> Resurs lås fungerar inte för ändringar som gjorts av användare som ansluter med hjälp av Cassandra SDK, CQL Shell eller Azure Portal om inte Cosmos DB-kontot först är låst med `disableKeyBasedMetadataWriteAccess` egenskapen aktive rad. Mer information om hur du aktiverar den här egenskapen finns i [förhindra ändringar från SDK](../../../role-based-access-control.md#prevent-sdk-changes): er.

## <a name="sample-script"></a>Exempelskript

[!code-azurecli-interactive[main](../../../../../cli_scripts/cosmosdb/cassandra/lock.sh "Create a resource lock for an Azure Cosmos DB Cassandra API keyspace, and table.")]

## <a name="script-explanation"></a>Förklaring av skript

Det här skriptet använder följande kommandon. Varje kommando i tabellen länkar till kommandospecifik dokumentation.

| Kommando | Kommentarer |
|---|---|
| [AZ lås skapa](/cli/azure/lock#az-lock-create) | Skapar ett lås. |
| [AZ lås lista](/cli/azure/lock#az-lock-list) | Visa information om lås. |
| [AZ lås show](/cli/azure/lock#az-lock-show) | Visa lås egenskaper. |
| [AZ lås borttagning](/cli/azure/lock#az-lock-delete) | Tar bort ett lås. |

## <a name="next-steps"></a>Nästa steg

-[Lås resurser för att förhindra oväntade ändringar](../../../../azure-resource-manager/management/lock-resources.md)

-[Azure Cosmos DB CLI-dokumentation](/cli/azure/cosmosdb).

-[Azure Cosmos DB CLI GitHub-lagringsplatsen](https://github.com/Azure-Samples/azure-cli-samples/tree/master/cosmosdb).
