---
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: include
ms.date: 09/01/2020
ms.author: tomfitz
ms.openlocfilehash: 949118214851c3eceffd8c1d638a4093bdf7f366
ms.sourcegitcommit: 58d3b3314df4ba3cabd4d4a6016b22fa5264f05a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 09/02/2020
ms.locfileid: "89303961"
---
| Resurs | Gräns |
| --- | --- |
| Resurser per [resurs grupp](../articles/azure-resource-manager/management/overview.md#resource-groups) | Resurserna begränsas inte av resurs gruppen. De är i stället begränsade av resurs typen i en resurs grupp. Se nästa rad. |
| Resurser per resurs grupp, per resurs typ |800 – vissa resurs typer kan överskrida gränsen på 800. Se [resurser som inte är begränsade till 800 instanser per resurs grupp](../articles/azure-resource-manager/management/resources-without-resource-group-limit.md). |
| Distributioner per resurs grupp i distributions historiken |800<sup>1</sup> |
| Resurser per distribution |800 |
| Hanterings lås per unik [omfattning](../articles/azure-resource-manager/management/overview.md#understand-scope)  |20 |
| Antal Taggar per resurs eller resurs grupp |50 |
| Etikett nyckel längd |512 |
| Tagg värde längd |256 |

<sup>1</sup> Distributioner tas automatiskt bort från historiken när du närmar dig gränsen. Om du tar bort en post från distributions historiken påverkas inte de distribuerade resurserna. Mer information finns i [automatiska borttagningar från distributions historiken](../articles/azure-resource-manager/templates/deployment-history-deletions.md).

#### <a name="template-limits"></a>Mall gränser

| Värde | Gräns |
| --- | --- |
| Parametrar |256 |
| Variabler |256 |
| Resurser (inklusive antal exemplar) |800 |
| Utdata |64 |
| Mall uttryck |24 576 tecken |
| Resurser i exporterade mallar |200 |
| Mallens storlek |4 MB |
| Parameter fil storlek |64 kB |

Du kan överskrida vissa begränsningar för mallar genom att använda en kapslad mall. Mer information finns i [använda länkade mallar när du distribuerar Azure-resurser](../articles/azure-resource-manager/templates/linked-templates.md). Om du vill minska antalet parametrar, variabler eller utdata kan du kombinera flera värden i ett objekt. Mer information finns i [objekt som parametrar](../articles/azure-resource-manager/resource-manager-objects-as-parameters.md).
