---
title: Skapa en Azure Kubernetes service-klusterresurs
titleSuffix: Azure Cognitive Services
description: Lär dig hur du skapar en Azure Kubernetes service-resurs (AKS).
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 04/01/2020
ms.author: aahi
ms.openlocfilehash: e7f5b6f3685a94b5497784360f8f12b22fb95012
ms.sourcegitcommit: dccb85aed33d9251048024faf7ef23c94d695145
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/28/2020
ms.locfileid: "87298800"
---
## <a name="create-an-azure-kubernetes-service-cluster-resource"></a>Skapa en Azure Kubernetes service-klusterresurs

1. Gå till [Azure Kubernetes-tjänsten](https://ms.portal.azure.com/#create/microsoft.aks)och välj **skapa**.

1. På fliken **grundläggande** anger du följande information:

    |Inställningen|Värde|
    |--|--|
    |Prenumeration|Välj en lämplig prenumeration.|
    |Resursgrupp|Välj en tillgänglig resursgrupp.|
    |Kluster namn för Kubernetes|Ange ett namn (gemener).|
    |Region|Välj en närliggande plats.|
    |Kubernetes-version|Vilket värde som är markerat som **(standard)**.|
    |DNS-namn-prefix|Skapas automatiskt, men du kan åsidosätta.|
    |Node-storlek|Standard DS2 v2:<br>`2 vCPUs`, `7 GB`|
    |Antal noder|Lämna skjutreglaget till standardvärdet.|

1. På fliken **noder** lämnar du **virtuella noder** och **VM Scale** set till standardvärdena.
1. På fliken **autentisering** lämnar du **tjänstens huvud namn** och **aktiverar RBAC** -inställningen till sina standardvärden.
1. På fliken **nätverk** anger du följande alternativ:

    |Inställningen|Värde|
    |--|--|
    |Routning av HTTP-program|Nej|
    |Nätverks konfiguration|Basic|

1. Kontrol lera att **behållar övervakning** har angetts till **aktive rad**på fliken **integrationer** och lämna **Log Analytics arbets yta** som standardvärde.
1. På fliken **taggar** lämnar du namn/värde-par tomma för tillfället.
1. Välj **Granska och skapa**.
1. När verifieringen har godkänts väljer du **skapa**.

> [!NOTE]
> Om verifieringen Miss lyckas kan det bero på ett "tjänstens huvud namn"-fel. Gå tillbaka till fliken **autentisering** och gå sedan tillbaka till **Granska + skapa**, där validering ska köras och skicka sedan.
