---
title: Azure Resource Manager vCPU-begäran om kvot ökning
description: Azure Resource Manager vCPU-begäran om kvot ökning
author: sowmyavenkat86
ms.author: svenkat
ms.date: 01/27/2020
ms.topic: how-to
ms.service: azure-supportability
ms.assetid: ce37c848-ddd9-46ab-978e-6a1445728a3b
ms.openlocfilehash: 7fd04428edc28315e714b9e81af0bea0e9344e52
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/02/2020
ms.locfileid: "84763813"
---
# <a name="quota-increase-requests"></a>Begäranden om att öka kvoten

Resource Manager-vCPU kvoter för virtuella datorer och skalnings uppsättningar för virtuella datorer tillämpas på två nivåer för varje prenumeration i varje region.

Den första nivån är den totala regionala virtuella processorer-gränsen för alla VM-serier. Den andra nivån är virtuella processorer-gränsen per virtuell dator serie, till exempel D-seriens virtuella processorer. När en ny virtuell dator ska distribueras måste summan av den nya och befintliga virtuella processorer-användningen för den virtuella dator serien inte överskrida vCPU-kvoten som har godkänts för den aktuella VM-serien. Dessutom får det totala antalet nya och befintliga vCPU som distribueras över alla VM-serien inte överstiga den totala regionala virtuella processorer-kvoten som har godkänts för prenumerationen. Om någon av dessa kvoter överskrids tillåts inte distributionen av virtuella datorer.
Du kan begära en ökning av virtuella processorer kvot gräns för VM-serien från Azure Portal. En ökning av kvoten för VM-serien ökar automatiskt den totala regionala virtuella processorer-gränsen med samma belopp.

När en ny prenumeration skapas får den totala regionala virtuella processorer inte vara lika med summan av standard kvoter för vCPU för alla enskilda VM-serier. Detta faktum kan leda till en prenumeration med tillräcklig kvot för varje enskild VM-serie som du vill distribuera. Det kunde inte ha tillräcklig kvot för den totala regionala virtuella processorer för alla distributioner. I så fall måste du skicka en begäran om att öka den totala regionala virtuella processorer-gränsen explicit. Den totala regionala virtuella processorer-gränsen får inte överskrida summan av den godkända kvoten över alla VM-serier för regionen.

Mer information om kvoter finns i [vCPU kvoter för virtuella datorer](../../virtual-machines/windows/quotas.md) och [begränsningar, kvoter och begränsningar för Azure-prenumerationer och tjänster](../../azure-resource-manager/management/azure-subscription-service-limits.md).

