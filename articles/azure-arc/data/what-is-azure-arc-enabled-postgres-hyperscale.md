---
title: Vad är Azure Arc aktiverat PostgreSQL-skalning?
description: Vad är Azure Arc aktiverat PostgreSQL-skalning?
services: azure-arc
ms.service: azure-arc
ms.subservice: azure-arc-data
author: TheJY
ms.author: jeanyd
ms.reviewer: mikeray
ms.date: 09/22/2020
ms.topic: how-to
ms.openlocfilehash: 7e8746d9b29b0b4af30ba799628328acc3fd8bc0
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 09/22/2020
ms.locfileid: "90941541"
---
# <a name="what-is-azure-arc-enabled-postgresql-hyperscale"></a>Vad är Azure Arc aktiverat PostgreSQL-skalning?

Azure Arc Enabled PostgreSQL-skalning är en av databas tjänsterna som är tillgängliga som en del av Azure Arc-aktiverade data tjänster. Azure Arc gör det möjligt att köra Azure Data Services lokalt, vid gränsen och i offentliga moln med Kubernetes och den infrastruktur som du väljer. Det värde som är av Azure Arc-aktiverade data tjänster återledas runt:
- Alltid aktuell
- Elastisk skalning
- Själv tjänst etablering
- Enhetlig hantering
- Stöd för frånkopplat scenario

Mer information finns på:
- [Vad är Azure Arc-aktiverade data tjänster](overview.md)
- [Anslutningslägen och krav](connectivity.md)

[!INCLUDE [azure-arc-data-preview](../../../includes/azure-arc-data-preview.md)]

## <a name="compare-solutions"></a>Jämför lösningar

I det här avsnittet beskrivs hur Azure Arc-aktiverade PostgreSQL skiljer sig från Azure Database for PostgreSQL storskalig (citus)?

## <a name="azure-database-for-postgresql-hyperscale-citus"></a>Hyperskala (Citus) för Azure Database for PostgreSQL

:::image type="content" source="media/postgres-hyperscale/postgresql-hyperscale.png" alt-text="Azure SQL Database för PostgreSQL-storskalig (citus)":::

Detta är den storskaliga form faktorn för postgres-databasmotorn som är tillgänglig som databas som en tjänst i Azure (PaaS). Den drivs av citus-tillägget som möjliggör skalnings upplevelsen. I den här formulärs faktorn körs tjänsten i Microsoft-datacenter och drivs av Microsoft.

## <a name="azure-arc-enabled-postgresql-hyperscale"></a>Azure Arc Enabled PostgreSQL-skalning

:::image type="content" source="media/postgres-hyperscale/postgresql-hyperscale-arc.png" alt-text="Azure Arc Enabled PostgreSQL-skalning":::

Detta är den storskaliga form faktorn för postgres-databasmotorn som är tillgänglig med Azure Arc-aktiverade data tjänster. Den drivs också av citus-tillägget som möjliggör skalnings upplevelsen. I den här formulärs faktorn tillhandahåller våra kunder den infrastruktur som är värd för systemen och använder dem.

## <a name="next-steps"></a>Nästa steg
- **Skapa**
   > **Vill du bara testa saker?**  
   > Kom igång snabbt med [Azure Arc-rivstart med](https://github.com/microsoft/azure_arc#azure-arc-enabled-data-services) på Azure Kubernetes service (AKS), AWS elastisk Kubernetes service (EKS), Google Cloud Kubernetes Engine (GKE) eller i en virtuell Azure-dator.

   - **Fram**
      - [Installera klient verktyg](install-client-tools.md)
      - [Skapa data styrenheten för Azure-bågen](create-data-controller.md) (kräver att klient verktygen installeras först)
      - [Skapa en Azure Database for PostgreSQL storskalig Server grupp på Azure-bågen](create-postgresql-hyperscale-server-group.md) (du måste först skapa en Azure Arc-datakontrollant.)
- [**Läs mer om Azure Arc-aktiverade data tjänster**](https://azure.microsoft.com/services/azure-arc/hybrid-data-services)
- [**Läs om Azure-bågen**](https://aka.ms/azurearc)
